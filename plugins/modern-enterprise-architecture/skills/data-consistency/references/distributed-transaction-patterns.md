# 分布式事务模式：Saga、2PC 和其他

## 概述

在分布式系统中，单个数据库的事务 ACID 保证不再有效。我们需要新的模式来处理跨多个服务/数据库的业务事务。

---

## 问题设定

### 业务场景：转账

```
账户 A 向账户 B 转 100 元

单个数据库中：
  BEGIN TRANSACTION
    UPDATE accounts SET balance = balance - 100 WHERE account_id = A
    UPDATE accounts SET balance = balance + 100 WHERE account_id = B
  COMMIT

要么都成功，要么都失败，ACID 保证。

分布式场景：账户分别在不同的服务/数据库中
  ├─ 账户 A 在 Service A（DB A）
  └─ 账户 B 在 Service B（DB B）

问题：如何保证两个操作的一致性？
  ├─ 如果 Service A 成功，Service B 失败 → 钱凭空消失
  ├─ 如果 Service A 失败，Service B 成功 → 钱凭空出现
  ├─ 如果都失败 → 钱消失
```

---

## 模式 1：两阶段提交（2PC）

### 原理

```
Coordinator（协调者）掌控整个过程

Phase 1：询问（Voting Phase）
  ├─ Coordinator 向所有 Participant 询问："能提交吗？"
  ├─ 各 Participant 检查条件
  │  ├─ 能：回答 "YES，我已准备好"（但还没提交）
  │  └─ 不能：回答 "NO"
  ├─ Participant 持有锁，等待 Coordinator 的命令

Phase 2：提交（Commit Phase）
  如果所有都回答 YES：
    ├─ Coordinator 发送 COMMIT 命令
    └─ 所有 Participant 提交本地事务

  如果任何一个回答 NO：
    ├─ Coordinator 发送 ROLLBACK 命令
    └─ 所有 Participant 回滚本地事务
```

### 例子：转账 100 元

```
初始状态：A=1000, B=2000

Step 1: 用户发起转账请求到 Coordinator

Step 2: Coordinator Phase 1（投票）
  ├─ 问 Service A："能从账户 A 扣 100 吗？"
  │  └─ 检查：A.balance (1000) >= 100 ✅
  │  └─ 回答：YES（锁定 A，预留 100）
  │
  └─ 问 Service B："能给账户 B 加 100 吗？"
     └─ 检查：有空间吗？✅
     └─ 回答：YES（锁定 B，预留空间）

Step 3: Coordinator Phase 2（提交）
  ├─ 所有都说 YES
  ├─ Coordinator 发送 COMMIT
  ├─ Service A 提交：A = 1000 - 100 = 900（释放锁）
  └─ Service B 提交：B = 2000 + 100 = 2100（释放锁）

最终状态：A=900, B=2100 ✅
```

### 故障场景

```
假设 Service B 故障（网络中断）

Step 1-2：同上

Step 3: Coordinator Phase 2
  ├─ Service A 提交成功
  ├─ Service B 无响应（网络中断）
  ├─ Coordinator 等待超时
  ├─ Coordinator 回滚：发送 ROLLBACK 到 Service A
  └─ Service A 回滚：A = 1000（恢复到原值）

最终状态：A=1000, B=2000（都未变化）✅
```

### 优缺点

```
优点：
  ✅ 保证强一致性
  ✅ 实现相对简单
  ✅ 有现成的系统支持（MySQL XA, Oracle）

缺点：
  ❌ 性能差：需要 Voting 和 Commit 两个阶段
  ❌ 可用性低：任何一个 Participant 故障都阻塞整个事务
  ❌ 网络分区：如果 Coordinator 与 Participant 通信中断，会长期阻塞
  ❌ 热点问题：Coordinator 成为瓶颈

性能指标：
  ├─ 吞吐量：100-1000 TPS（取决于配置）
  ├─ 延迟：100-1000ms（两个阶段的通信延迟）
  ├─ 适合场景：小规模系统，对一致性要求极高
```

### 实现建议

```
使用场景：
  ├─ 不适合互联网应用（太慢）
  └─ 适合金融系统（要求强一致）

技术选择：
  ├─ 数据库内置：MySQL XA, PostgreSQL
  ├─ 分布式事务中间件：DTM, HMILY
  ├─ 区块链：自动 2PC 机制

最佳实践：
  ✅ 事务范围要小（快速完成）
  ✅ 参与者数量要少（< 3 个）
  ✅ 超时设置要合理
  ✅ 要有补偿机制（处理失败）
```

---

## 模式 2：Saga 模式

### 原理

```
放弃分布式事务，改用一系列本地事务 + 补偿机制

核心思想：
  ├─ 将长事务分解为一系列短的本地事务
  ├─ 每个本地事务都是自己数据库的 ACID 事务
  ├─ 如果某个步骤失败，向前回滚
  └─ 通过补偿操作恢复到一致状态
```

### 例子：转账 100 元

```
Saga 流程：

Step 1: Service A 扣款
  ├─ 本地事务：UPDATE accounts SET balance = balance - 100 WHERE id = A
  ├─ 成功 ✅
  └─ Compensate 操作：UPDATE accounts SET balance = balance + 100 WHERE id = A

Step 2: Service B 收款
  ├─ 本地事务：UPDATE accounts SET balance = balance + 100 WHERE id = B
  ├─ 失败（如账户已关闭）❌
  └─ 触发补偿

补偿流程：
  ├─ 执行 Step 1 的 Compensate 操作
  └─ Service A 账户恢复 1000

最终状态：A=1000, B=2000（都未变化）✅
```

### 两种实现方式

#### 方式 1：编程型 Saga（Choreography）

```
各个服务互相通过消息/事件通信

流程：
  Service A
    ├─ 执行：扣款
    ├─ 成功 → 发送事件 "TransferDebited"
    └─ 失败 → 发送事件 "TransferFailed"

Service B（监听 TransferDebited 事件）
    ├─ 执行：收款
    ├─ 成功 → 发送事件 "TransferCredited"
    └─ 失败 → 发送事件 "TransferFailed"

Service A（监听 TransferFailed 事件）
    ├─ 执行补偿：退款
    └─ 恢复初始状态

优点：
  ✅ 去中心化，无单点故障
  ✅ 性能好（异步处理）

缺点：
  ❌ 逻辑分散，难以理解
  ❌ 循环依赖风险
  ❌ 调试困难
```

#### 方式 2：协调型 Saga（Orchestration）

```
由 Saga Orchestrator 协调整个流程

Orchestrator
  ├─ Step 1: 调用 Service A.debit()
  │   ├─ 成功？继续
  │   └─ 失败？执行补偿
  │
  ├─ Step 2: 调用 Service B.credit()
  │   ├─ 成功？事务完成
  │   └─ 失败？执行补偿链
  │
  └─ 补偿链：Service A.refund()

优点：
  ✅ 逻辑清晰集中
  ✅ 易于理解和维护
  ✅ 便于调试

缺点：
  ❌ Orchestrator 成为瓶颈
  ❌ 需要额外的协调服务
```

### Saga vs 2PC 对比

```
特性            | 2PC   | Saga
─────────────────┼──────┼─────────
实时一致性        | ✅   | ⏳ 最终一致
性能            | ⬇️ 低 | ⬆️ 高
可用性          | ⬇️ 低 | ⬆️ 高
实现复杂度        | 中   | 高
网络分区容错      | ❌   | ✅
适合场景         | 金融 | 电商

应该使用 2PC：
  └─ 需要即时强一致

应该使用 Saga：
  └─ 接受最终一致，追求高可用
```

---

## 模式 3：事件溯源（Event Sourcing）

### 原理

```
不保存数据的当前状态，而是保存所有事件历史

概念：
  ├─ 每个操作都产生一个事件
  ├─ 事件是不可变的
  ├─ 当前状态 = 回放所有事件

例子：账户余额
  初始状态：无
  事件 1: AccountCreated(id=A, balance=1000)
  事件 2: MoneyDebited(amount=100)  ← 转账
  事件 3: MoneyDebited(amount=50)   ← 取现

  当前余额 = 1000 - 100 - 50 = 850
```

### 优缺点

```
优点：
  ✅ 完整的审计日志
  ✅ 支持时间旅行（查看任何时刻的状态）
  ✅ 支持事件回放（从任何点重建状态）
  ✅ 天然支持最终一致性

缺点：
  ❌ 存储空间大（需要保存所有事件）
  ❌ 复杂的查询（需要回放事件）
  ❌ 事件版本管理复杂
  ❌ 学习曲线陡峭
```

---

## 模式 4：TCC 模式（补偿型事务）

### 原理

```
Try - Confirm - Cancel

Try 阶段：预留资源
  └─ 检查条件，预留资源（但不提交）

Confirm 阶段：提交业务操作
  └─ 资源已预留，现在提交

Cancel 阶段：补偿操作
  └─ 如果失败，撤销预留

与 Saga 的区别：
  ├─ Saga：先执行，失败再补偿
  └─ TCC：先预留，后提交，最后才能补偿
```

### 例子：转账

```
Try 阶段：
  ├─ Service A 检查：能扣款吗？
  │  └─ 冻结 100 元（不实际扣）
  └─ Service B 检查：能收款吗？
     └─ 预留空间（不实际加）

Confirm 阶段：
  ├─ Service A 扣款：冻结 100 → 实际扣款
  └─ Service B 收款：预留空间 → 实际加款

Cancel 阶段（如果失败）：
  ├─ Service A 取消冻结
  └─ Service B 取消预留
```

### 优缺点

```
优点：
  ✅ 强一致性（比 Saga 更强）
  ✅ 实现相对清晰

缺点：
  ❌ Try 阶段要检查和冻结，复杂度高
  ❌ 资源占用期间长
  ❌ 业务流程改动大
```

---

## 选择指南

```
Q: 需要强一致性吗？
├─ 是（金融转账）→ 考虑 2PC 或 TCC
└─ 否（电商）→ 考虑 Saga

Q: 是否已有事件驱动架构？
├─ 是 → Saga(Choreography) + Event Sourcing
└─ 否 → Saga(Orchestration)

Q: 参与的服务数多吗？
├─ <= 3 个 → 2PC 可以接受
└─ > 3 个 → Saga（2PC 会很慢）

Q: 能接受多大的一致性窗口？
├─ 毫秒 → 2PC
├─ 秒 → Saga + 异步补偿
└─ 分钟 → Event Sourcing

推荐组合：
  电商 → Saga(Orchestration) + 监控
  金融 → 2PC + 补偿 或 TCC
  大数据 → Event Sourcing + 实时处理
```

---

## 实施建议

```
1. 明确一致性需求
   ✅ 什么数据必须强一致？
   ✅ 什么数据可以最终一致？

2. 设计补偿操作
   ✅ 每个操作都要有补偿
   ✅ 补偿要幂等
   ✅ 要能处理补偿失败

3. 建立监控和告警
   ✅ 监控事务状态
   ✅ 及时发现失败
   ✅ 自动恢复机制

4. 文档化设计
   ✅ 记录所有操作
   ✅ 记录补偿规则
   ✅ 记录故障处理
```

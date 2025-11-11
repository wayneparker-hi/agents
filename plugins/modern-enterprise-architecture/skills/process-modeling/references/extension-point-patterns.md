# 扩展点模式详解

## 概述

扩展点是业务流程中预留的、允许灵活插入额外功能或逻辑的地点。以下 6 种模式是企业应用中最常见的扩展点实现方式。

---

## 模式 1：前置条件检查（Pre-Condition Check）

### 定义

在执行主流程前，插入检查逻辑。根据检查结果决定是否继续、修改参数或拒绝操作。

### 场景

```
订单创建流程
  ↓
[扩展点] 风险检查
  - 检查订单是否来自黑名单用户
  - 检查订单金额是否异常
  - 检查收货地址是否有高风险标志
  ↓
（继续创建订单 或 拒绝）
```

### 实现方式

**配置型**：
```java
OrderCreationProcess {
    process() {
        // 核心流程前的检查点
        for (PreConditionChecker checker : preConditionCheckers) {
            if (!checker.check(order)) {
                throw new OrderRejected(checker.getReason());
            }
        }

        // 核心流程
        createOrder(order);
    }
}
```

**事件驱动型**：
```java
public void createOrder(Order order) {
    // 发布事件，触发所有订阅者的检查
    eventBus.publish(new OrderCreationRequested(order));

    // 等待所有检查完成
    checkResult result = waitForChecks(order);

    if (result.isPassed()) {
        persistOrder(order);
    } else {
        rejectOrder(order, result.getReason());
    }
}
```

### 优势 vs 劣势

```
✅ 优势：
- 逻辑清晰，执行流程可预测
- 容易理解和维护

❌ 劣势：
- 检查器增多时性能下降
- 检查器间可能有依赖或冲突
```

### 配置参数

```yaml
PreConditionChecks:
  - name: BlacklistCheck
    enabled: true
    timeout: 1000ms

  - name: AmountAnomalyCheck
    enabled: true
    threshold: 10000

  - name: RiskAddressCheck
    enabled: true
```

---

## 模式 2：后置操作处理器（Post-Operation Handler）

### 定义

核心流程执行完成后，触发一系列后续操作。这些操作彼此独立，可以异步执行。

### 场景

```
订单确认
  ↓
[核心] 持久化订单
  ↓
[扩展点] 后置处理
  - 发送确认邮件
  - 更新库存
  - 触发发货流程
  - 积分增加
  - 发送短信通知
```

### 实现方式

**事件驱动型**（推荐）：
```java
public void confirmOrder(Order order) {
    // 1. 核心流程
    order.confirm();
    orderRepository.save(order);

    // 2. 发布事件，触发所有后置处理
    eventBus.publish(new OrderConfirmedEvent(order));

    // 异步处理，不影响主流程
    // 监听者：
    // - EmailService.onOrderConfirmed()
    // - InventoryService.onOrderConfirmed()
    // - ShipmentService.onOrderConfirmed()
    // - LoyaltyService.onOrderConfirmed()
}
```

**Callback 型**：
```java
public void confirmOrder(Order order) {
    order.confirm();
    orderRepository.save(order);

    // 顺序执行后置操作
    for (OrderConfirmationHandler handler : postHandlers) {
        try {
            handler.handle(order);
        } catch (Exception e) {
            logger.error("Handler failed: " + handler.getName(), e);
            // 继续处理下一个
        }
    }
}
```

### 优势 vs 劣势

```
✅ 优势：
- 核心流程和后续操作解耦
- 可以异步执行，提高性能
- 后处理器失败不影响主流程

❌ 劣势：
- 最终一致性，不能保证所有后处理成功
- 需要处理失败情况（重试、补偿）
```

### 配置参数

```yaml
PostOperationHandlers:
  - name: SendConfirmationEmail
    async: true
    retryCount: 3
    retryDelay: 5000ms

  - name: UpdateInventory
    async: false  # 库存更新必须同步
    retryCount: 5

  - name: TriggerShipment
    async: true
    priority: HIGH
```

---

## 模式 3：策略选择（Strategy Selection）

### 定义

根据运行时的条件，选择不同的处理策略（算法）来完成同一个任务。

### 场景

```
计算订单邮费
  ↓
[扩展点] 选择邮费计算策略
  ├─ StandardStrategy（标准邮费）
  ├─ VIPStrategy（VIP 免邮）
  ├─ FreeShippingStrategy（满额包邮）
  ├─ RegionalStrategy（按地区定价）
  └─ PartnerStrategy（合作商特价）
  ↓
执行选定的策略
```

### 实现方式

```java
// 策略接口
public interface ShippingFeeStrategy {
    Money calculate(Order order);
}

// 具体策略
public class StandardStrategy implements ShippingFeeStrategy {
    public Money calculate(Order order) {
        return baseFee.multiply(distance);
    }
}

public class VIPStrategy implements ShippingFeeStrategy {
    public Money calculate(Order order) {
        return Money.ZERO;  // VIP 免邮
    }
}

// 选择器
public class ShippingFeeCalculator {
    private final Map<String, ShippingFeeStrategy> strategies;

    public Money calculate(Order order) {
        // 策略选择逻辑
        String strategyKey = selectStrategy(order);
        ShippingFeeStrategy strategy = strategies.get(strategyKey);

        return strategy.calculate(order);
    }

    private String selectStrategy(Order order) {
        if (order.isVIP()) {
            return "VIP";
        } else if (order.getTotalAmount() > 500) {
            return "FreeShipping";
        } else if (isPartnerRegion(order.getRegion())) {
            return "Partner";
        } else {
            return "Standard";
        }
    }
}
```

### 策略选择规则

```yaml
StrategySelection:
  ShippingFee:
    - condition: "user.level == VIP"
      strategy: VIPStrategy
      priority: 1

    - condition: "order.amount > 500 && region != restricted"
      strategy: FreeShippingStrategy
      priority: 2

    - condition: "isPartnerRegion(region)"
      strategy: PartnerStrategy
      priority: 3

    - condition: "else"
      strategy: StandardStrategy
      priority: 999
```

### 优势 vs 劣势

```
✅ 优势：
- 易于添加新策略
- 运行时灵活切换
- 代码符合开闭原则

❌ 劣势：
- 策略过多时选择逻辑复杂
- 需要统一的策略接口定义
```

---

## 模式 4：条件路由（Conditional Routing）

### 定义

根据条件，将流程路由到不同的分支。每条分支是独立的处理流程。

### 场景

```
处理订单
  ↓
[扩展点] 路由点：根据商品类型
  ├─ 实物商品 → 仓库出库流程
  ├─ 虚拟商品 → 自动发送流程
  ├─ 预售商品 → 等待发货流程
  └─ 团购商品 → 等待成团流程
  ↓
分别处理
```

### 实现方式

**配置驱动**：
```yaml
ProcessRouting:
  OrderProcessing:
    - condition: "product.type == PHYSICAL"
      handler: WarehouseHandler

    - condition: "product.type == VIRTUAL"
      handler: InstantDeliveryHandler

    - condition: "product.type == PRESALE"
      handler: WaitForReleaseHandler

    - condition: "product.type == GROUPBUY"
      handler: GroupBuyHandler
```

**代码实现**：
```java
public class OrderProcessingRouter {
    private final Map<ProductType, ProcessHandler> handlers;

    public void process(Order order) {
        for (OrderLine line : order.getLines()) {
            ProductType type = line.getProduct().getType();
            ProcessHandler handler = handlers.get(type);

            handler.handle(line);
        }
    }
}

// 各处理器实现
public class WarehouseHandler implements ProcessHandler {
    public void handle(OrderLine line) {
        // 仓库出库逻辑
        inventory.reduce(line.getProductId(), line.getQuantity());
        warehouse.createPickingTask(line);
    }
}

public class InstantDeliveryHandler implements ProcessHandler {
    public void handle(OrderLine line) {
        // 虚拟商品自动发送
        sendVirtualProduct(line);
    }
}
```

### 优势 vs 劣势

```
✅ 优势：
- 不同流程分离，各自优化
- 易于理解每条分支的逻辑
- 便于测试

❌ 劣势：
- 多条分支维护成本高
- 分支间可能有重复逻辑
```

---

## 模式 5：并行处理（Parallel Processing）

### 定义

多个任务可以并行执行，无需等待前一个完成。适合任务间独立无依赖的场景。

### 场景

```
订单完成后
  ↓
[扩展点] 并行执行
  ├─ Task 1: 发送确认邮件
  ├─ Task 2: 更新积分
  ├─ Task 3: 发起发票申请
  └─ Task 4: 更新用户统计
  ↓
（所有任务完成后标记为完成）
```

### 实现方式

**Future / Promise 型**：
```java
public void completeOrder(Order order) {
    List<CompletableFuture<?>> futures = new ArrayList<>();

    // 并行任务 1: 发送邮件
    futures.add(CompletableFuture.runAsync(() -> {
        emailService.sendOrderCompletionEmail(order);
    }));

    // 并行任务 2: 更新积分
    futures.add(CompletableFuture.runAsync(() -> {
        loyaltyService.addPoints(order);
    }));

    // 并行任务 3: 申请发票
    futures.add(CompletableFuture.runAsync(() -> {
        invoiceService.createInvoice(order);
    }));

    // 等待所有任务完成
    CompletableFuture.allOf(futures.toArray(new CompletableFuture[0]))
        .thenAccept(v -> {
            order.markAsFullyCompleted();
            orderRepository.save(order);
        });
}
```

**消息队列型**：
```java
public void completeOrder(Order order) {
    // 发布事件
    eventBus.publish(new OrderCompletedEvent(order));

    // 多个消费者并行处理
    // - EmailConsumer 监听事件，异步发送邮件
    // - LoyaltyConsumer 监听事件，异步更新积分
    // - InvoiceConsumer 监听事件，异步申请发票
}
```

### 配置参数

```yaml
ParallelTasks:
  - name: SendEmail
    timeout: 5000ms
    threadPool: 10

  - name: UpdateLoyalty
    timeout: 3000ms
    threadPool: 5

  - name: CreateInvoice
    timeout: 10000ms
    threadPool: 3

  failureStrategy: CONTINUE  # 某个任务失败是否继续
```

### 优势 vs 劣势

```
✅ 优势：
- 总体处理时间最短
- 任务间独立，易于扩展
- 充分利用多核优势

❌ 劣势：
- 复杂的线程管理
- 错误处理和重试困难
- 难以理解执行顺序（对调试不友好）
```

---

## 模式 6：补偿机制（Compensation Mechanism）

### 定义

如果后续步骤失败，执行补偿操作来撤销已完成的步骤，保持数据一致性。

### 场景

```
转账流程
  ↓
Step 1: 从账户 A 扣款  ✓
  ↓
[扩展点] 补偿
  如果失败：恢复账户 A

Step 2: 向账户 B 入账  ✗ 失败
  ↓
[补偿] 执行恢复操作
  反向扣款 → 账户 A 入账
```

### 实现方式

**显式补偿**：
```java
public void transfer(Account from, Account to, Money amount) {
    // Step 1: 从源账户扣款
    from.debit(amount);
    accountRepository.save(from);

    try {
        // Step 2: 向目标账户入账
        to.credit(amount);
        accountRepository.save(to);
    } catch (Exception e) {
        // 补偿：恢复源账户
        from.credit(amount);  // 反向操作
        accountRepository.save(from);

        throw new TransferFailedException(e);
    }
}
```

**Saga 模式**（长事务）：
```java
public class TransferSaga {
    public void execute(String fromAccountId, String toAccountId, Money amount) {
        // Saga Step 1: 扣款
        String debitTxnId = accountService.debit(fromAccountId, amount);

        try {
            // Saga Step 2: 入账
            String creditTxnId = accountService.credit(toAccountId, amount);

            // 成功标记
            sagaLog.markSuccess(fromAccountId, toAccountId);

        } catch (Exception e) {
            // 补偿：回滚 Saga Step 1
            accountService.compensateDebit(debitTxnId);

            sagaLog.markFailed(fromAccountId, toAccountId, e);
            throw e;
        }
    }
}
```

**Outbox 模式**（保证消息可靠性）：
```java
@Transactional
public void transfer(Account from, Account to, Money amount) {
    // Step 1 & 2 在同一个事务中
    from.debit(amount);
    to.credit(amount);

    accountRepository.save(from);
    accountRepository.save(to);

    // Step 3: 在同一个事务中记录到 Outbox
    TransferOutbox outbox = new TransferOutbox(from.getId(), to.getId(), amount);
    outboxRepository.save(outbox);

    // 异步发送事件，失败自动重试
    // TransferCompletedEvent 会从 Outbox 中拉取
}
```

### 补偿规则

```yaml
CompensationRules:
  Transfer:
    - step: 1_Debit
      compensation: 1_Debit_Rollback
      condition: "step_2_failed"

    - step: 2_Credit
      compensation: 2_Credit_Rollback
      condition: "step_3_failed"
```

### 优势 vs 劣势

```
✅ 优势：
- 无分布式锁，高性能
- 长流程可靠性高
- 补偿逻辑清晰

❌ 劣势：
- 补偿操作复杂
- 幂等性要求高
- 中间状态可见（需要业务理解）
```

---

## 模式对比总结

| 模式 | 何时使用 | 执行特点 | 复杂度 |
|------|---------|---------|-------|
| 前置条件检查 | 需要预先验证 | 串行 | ⭐ |
| 后置操作处理 | 多个独立后续操作 | 可异步 | ⭐⭐ |
| 策略选择 | 相同任务多种算法 | 串行 | ⭐ |
| 条件路由 | 不同类型走不同流程 | 串行或并行 | ⭐⭐⭐ |
| 并行处理 | 任务独立无依赖 | 并行 | ⭐⭐⭐⭐ |
| 补偿机制 | 多步操作需要回滚 | 串行+补偿 | ⭐⭐⭐⭐⭐ |

---

## 关键建议

1. **模式选择**：优先使用前置检查和后置处理，只在必要时使用复杂模式
2. **可观测性**：记录所有扩展点的执行情况，便于调试
3. **性能**：异步化后置操作，减少同步等待
4. **一致性**：对于关键流程，使用补偿机制或 Outbox 模式保证可靠性
5. **可维护性**：扩展点数量不超过 5 个，否则流程过于复杂

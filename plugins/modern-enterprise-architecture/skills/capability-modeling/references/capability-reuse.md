# 能力复用与组件库建设

## 能力复用的价值

### 为什么要复用？

```
不复用：
├─ 相同功能重复开发 → 3x 开发时间
├─ 维护多份代码 → 维护成本高
├─ 不同版本差异 → 支持成本高
└─ 团队效率低 → 进度慢

复用：
├─ 代码一次开发，多处使用 → 开发效率 3x
├─ 统一维护 → 维护成本低
├─ 同一版本 → 问题一次解决
└─ 团队协作 → 进度快
```

### 复用的三个层次

```
L1 层：能力复用（跨域）
  不同业务域使用相同的业务能力
  例：订单域和退货域都需要"获取用户信息"能力

L2 层：接口复用（跨应用）
  多个应用通过统一接口调用
  例：支付接口被订单应用和营销应用都使用

L3 层：组件复用（跨服务）
  多个服务共享同一个组件库
  例：日期工具、金钱计算都被多个服务复用
```

---

## 识别可复用的能力

### 识别标准

```
✅ 适合复用的能力
├─ 功能相同或相似（都是查询用户）
├─ 使用频繁（> 3 个地方）
├─ 相对稳定（需求变化不大）
├─ 接口清晰（易于集成）
└─ 业务价值高（节省成本大）

❌ 不适合复用的能力
├─ 功能特殊化（专属某个流程）
├─ 使用不频繁（只有 1-2 个地方）
├─ 变化频繁（需求变化大）
├─ 接口混乱（难以集成）
└─ 业务价值低（节省成本小）
```

### 识别流程

```
第 1 步：列举所有能力
订单域：[订单创建] [订单修改] [订单查询] [库存检查] ...
退货域：[退货申请] [退货审批] [退货查询] [库存检查] ...
积分域：[积分增加] [积分扣减] [积分查询] [用户查询] ...

第 2 步：找出相同的能力
[库存检查]：出现在订单域、退货域 → 可复用
[用户查询]：出现在积分域、订单域 → 可复用

第 3 步：评估复用价值
库存检查：使用频率：高，稳定性：高 → ✅ 优先复用
用户查询：使用频率：高，稳定性：高 → ✅ 优先复用

第 4 步：规划复用实现
独立服务：InventoryService
公用库：UserQueryService
```

---

## 复用的三种模式

### 模式 1：公共库（Shared Library）

**场景**：通用工具、基础组件

**优势**：
- 部署简单
- 性能最好（无网络调用）
- 本地调用，易于开发

**劣势**：
- 所有使用者需要更新依赖
- 版本管理困难
- 难以独立演进

**示例**：
```
┌─────────────────────────────────────────────┐
│ shared-library-core                         │
├─────────────────────────────────────────────┤
│ ├─ utils (日期、金钱、字符串等)             │
│ ├─ dto (数据传输对象)                       │
│ ├─ enums (枚举)                             │
│ └─ exceptions (异常)                        │
└─────────────────────────────────────────────┘
     ↑
     │ maven 依赖
     │
  ┌──┴──────────────┬──────────────┐
  │                 │              │
 Order           Payment       Inventory
 Service         Service       Service
```

### 模式 2：微服务（Microservice）

**场景**：独立的业务能力、跨域复用

**优势**：
- 独立部署和演进
- 版本管理清晰
- 支持多语言实现
- 易于扩展和优化

**劣势**：
- 网络调用开销
- 运维复杂
- 分布式事务困难
- 调试困难

**示例**：
```
┌──────────────────────────┐
│  User Service            │
│  (用户查询、管理)        │
└──────────────────────────┘
     ↑
     │ HTTP/gRPC
     │
  ┌──┴──────────────┬──────────────┬────────────┐
  │                 │              │            │
Order             Payment      Inventory    Marketing
Service           Service      Service      Service
```

### 模式 3：数据共享（Shared Database）

**场景**：简单数据查询、低延迟要求

**优势**：
- 实时性好
- 实现简单

**劣势**：
- 耦合度高
- 数据库压力大
- 难以独立演进

**示例**：
```
┌────────────────────────────┐
│  Shared Database           │
│  (用户、商品、库存等表)    │
└────────────────────────────┘
     ↑
     │ SQL 查询
     │
  ┌──┴──────────────┬──────────────┐
  │                 │              │
Order             Payment      Inventory
Service           Service      Service
```

---

## 建设可复用的能力

### 原则 1：接口优先

设计接口时，考虑多个使用场景。

**反例**（只考虑一个场景）：
```java
// ❌ 接口过于具体，不易复用
public interface OrderValidator {
    ValidationResult validateForOrderCreation(Order order);
    ValidationResult validateForOrderModification(Order order);
    ValidationResult validateForOrderCancellation(Order order);
}
```

**正例**（通用接口）：
```java
// ✅ 通用接口，多场景复用
public interface OrderValidator {
    ValidationResult validate(Order order, ValidationContext context);
}

public enum ValidationContext {
    ORDER_CREATION,
    ORDER_MODIFICATION,
    ORDER_CANCELLATION
}
```

### 原则 2：清晰的职责边界

一个可复用的能力，职责应该清晰且边界明确。

**反例**（职责混乱）：
```java
// ❌ 职责混乱，难以理解和复用
public interface UserService {
    User getUserById(String id);
    void updateUser(User user);
    void deleteUser(String id);
    void sendNotification(User user, String message);  // 为什么这个接口在这里？
    void logUserAction(User user, String action);      // 这个呢？
}
```

**正例**（职责清晰）：
```java
// ✅ 职责清晰，易于理解和复用
public interface UserRepository {
    User findById(String id);
    void save(User user);
    void delete(String id);
}

public interface NotificationService {
    void send(User user, String message);
}

public interface UserActionLogger {
    void log(User user, String action);
}
```

### 原则 3：最小化依赖

可复用的能力应该最小化对其他组件的依赖。

**反例**（依赖过多）：
```java
// ❌ 依赖过多，难以复用
public class OrderProcessor {
    private UserService userService;
    private InventoryService inventoryService;
    private PaymentService paymentService;
    private NotificationService notificationService;
    private LogisticsService logisticsService;
    private AnalyticsService analyticsService;

    // 想在别的地方用 OrderProcessor 的某个方法，
    // 却要注入 6 个依赖。。。太复杂了
}
```

**正例**（依赖最小化）：
```java
// ✅ 依赖最小化，易于复用
public class OrderValidator {
    private ValidationRules rules;

    // 只依赖验证规则，易于在任何地方使用
}

public class PriceCalculator {
    // 无依赖，完全无状态，最易复用
    public Money calculate(Order order, DiscountPolicy policy) {
        // ...
    }
}
```

### 原则 4：完善的异常处理

可复用的能力应该有明确的异常定义。

**反例**（异常不清晰）：
```java
// ❌ 异常混乱
public User getUserById(String id) throws Exception {
    // Exception 太通用，使用者不知道如何处理
}
```

**正例**（异常清晰）：
```java
// ✅ 异常清晰
public User getUserById(String id) throws UserNotFoundException {
    // 明确的异常，使用者知道如何处理
}
```

---

## 组件库的版本管理

### 版本号规则（Semantic Versioning）

```
版本格式：Major.Minor.Patch
└─ Major：重大变更，破坏兼容性
└─ Minor：添加功能，向后兼容
└─ Patch：修复 bug，向后兼容

示例：
1.0.0  → 初始版本
1.1.0  → 添加新功能，向后兼容
1.1.1  → 修复 bug
2.0.0  → 重大重构，不兼容
```

### 兼容性管理

```
发布流程：
1. 开发新功能
   v1.2.0 (新增接口)

2. 保留旧接口，标记过期
   @Deprecated(since = "1.2.0", forRemoval = true)
   public void oldMethod() { ... }

3. 提供新接口
   public void newMethod() { ... }

4. 在至少一个大版本后才移除旧接口
   v2.0.0 (移除旧接口)

5. 发布迁移指南
   "从 1.x 升级到 2.0: ..."
```

---

## 复用的度量

### 复用度指标

```
复用度 = (被复用的代码行数) / (总代码行数)

目标：
├─ 基础库：> 80% (高度复用)
├─ 工具库：> 60% (中等复用)
└─ 业务库：> 30% (适度复用)
```

### 维护成本指标

```
维护成本 = (修复 bug 的位置数) / (发布版本数)

理想：
├─ 通用库：< 1.5 (bug 修复在一个地方)
├─ 微服务：< 2.0 (bug 修复在一个服务)
└─ 非复用：> 3.0 (重复修复)
```

---

## 常见问题

### Q1: 什么时候应该开始复用？

```
❌ 不要太早复用：
- 功能不稳定时，强行复用导致频繁改动
- 使用场景不清楚时，设计过度复杂

✅ 合适的时机：
- 至少有 2 个地方使用相同逻辑
- 逻辑相对稳定（不经常变更）
- 有明确的使用场景
```

### Q2: 如何处理微小的差异？

```
❌ 为每个场景创建新的能力
- 复用度大幅下降
- 维护成本上升

✅ 使用参数或策略模式处理差异
public Money calculate(Order order, PricingStrategy strategy) {
    // 同一个方法，通过不同的策略处理差异
}
```

### Q3: 如何逐步演进复用能力？

```
第 1 阶段：识别和提取
- 找出多个地方重复的代码
- 提取为共享库或服务

第 2 阶段：标准化和文档
- 统一接口和数据模型
- 完善文档和示例

第 3 阶段：优化和度量
- 性能优化
- 建立度量体系

第 4 阶段：治理和演进
- 版本管理
- 定期评审
```

---

## 关键建议

1. **从简单开始**：先用共享库，再考虑微服务
2. **接口优先**：设计通用接口，满足多个场景
3. **最小化依赖**：可复用的能力依赖越少越好
4. **完善文档**：好的文档能加速复用
5. **版本管理**：明确的版本策略避免混乱
6. **定期评审**：定期评估复用效果和优化机会

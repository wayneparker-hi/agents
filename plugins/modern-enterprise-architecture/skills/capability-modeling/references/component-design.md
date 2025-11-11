# L3 组件设计指南

## 什么是好的 L3 组件？

L3 组件是实现 L2 能力的技术构件。好的组件设计直接影响系统的可维护性、可复用性和可扩展性。

---

## 组件的核心特征

### 1. 职责单一（Single Responsibility）

**原则**：每个组件只做一件事，且做好它。

**反例**：
```java
// ❌ 职责过重
public class OrderComponent {
    // 创建订单
    public void createOrder() { ... }

    // 计算价格
    public void calculatePrice() { ... }

    // 发送邮件
    public void sendEmail() { ... }

    // 调用库存服务
    public void checkInventory() { ... }

    // 更新数据库
    public void saveOrder() { ... }
}
```

**正例**：
```java
// ✅ 职责单一
public class OrderValidator {
    // 只负责订单验证
    public ValidationResult validate(Order order) { ... }
}

public class PriceCalculator {
    // 只负责价格计算
    public Money calculate(Order order) { ... }
}

public class OrderRepository {
    // 只负责数据持久化
    public void save(Order order) { ... }
}
```

### 2. 清晰的接口

**原则**：对外只暴露必要的接口，隐藏实现细节。

**接口设计规则**：
```java
// ✅ 好的接口
public interface OrderProcessor {
    // 清晰的方法名
    ProcessResult process(Order order);

    // 清晰的入参和返回值
    // 返回 Result 对象，包含成功/失败状态
}

// ❌ 不好的接口
public interface OrderService {
    // 方法过多，职责不清
    void process(Order order);
    void calculate(Order order);
    void validate(Order order);
    void store(Order order);
    void notify(Order order);
    void analyze(Order order);
    // ... 10+ 个方法
}
```

### 3. 清晰的数据模型

**原则**：组件的数据模型应该清晰、一致、无冗余。

**示例**：
```
OrderValidator
├─ Input: Order, ValidationRules
├─ Output: ValidationResult
│  ├─ isValid: boolean
│  ├─ errors: List<Error>
│  └─ warnings: List<Warning>
└─ 内部状态: None（无状态组件）
```

### 4. 独立性

**原则**：尽可能减少对其他组件的依赖。

**依赖对比**：
```
❌ 高耦合（难以复用和测试）
OrderProcessor
├─ 依赖：InventoryService
├─ 依赖：PaymentService
├─ 依赖：LogisticsService
├─ 依赖：NotificationService
└─ 依赖：AnalyticsService
结果：5 个依赖，改一个就影响全体

✅ 低耦合（易于复用和测试）
OrderValidator
├─ 依赖：ValidationRules（配置）
└─ 依赖：Order（数据）
结果：仅 2 个依赖，可单独测试和复用
```

---

## 组件的三种类型

### 类型 1：服务类组件（Service Component）

**定义**：提供业务逻辑处理。

**特征**：
- 有明确的业务能力
- 可能有状态（数据）
- 有多个方法实现不同业务逻辑

**示例**：
```java
// ✅ OrderValidationService
public class OrderValidationService {
    public ValidationResult validate(Order order) {
        // 验证逻辑
    }
}

// ✅ PriceCalculationService
public class PriceCalculationService {
    public Money calculate(Order order, DiscountPolicy policy) {
        // 计算逻辑
    }
}
```

### 类型 2：数据访问类组件（Data Access Component / Repository）

**定义**：处理数据持久化和查询。

**特征**：
- 操作数据
- 提供 CRUD 接口
- 对业务逻辑无感知

**示例**：
```java
// ✅ OrderRepository
public interface OrderRepository {
    Order findById(OrderId id);
    void save(Order order);
    void delete(OrderId id);
    List<Order> findByCustomer(CustomerId customerId);
}
```

### 类型 3：工具类组件（Utility Component）

**定义**：提供通用工具函数，无业务逻辑。

**特征**：
- 无状态
- 可被多个组件复用
- 通常是静态方法或简单函数

**示例**：
```java
// ✅ MoneyCalculator (工具类)
public class MoneyCalculator {
    public static Money add(Money a, Money b) { ... }
    public static Money subtract(Money a, Money b) { ... }
    public static Money multiply(Money a, BigDecimal factor) { ... }
}

// ✅ DateUtils (工具类)
public class DateUtils {
    public static boolean isBusinessDay(LocalDate date) { ... }
    public static LocalDate addBusinessDays(LocalDate date, int days) { ... }
}
```

---

## 组件间的依赖关系

### 依赖规则

```
✅ 允许的依赖
├─ Service → Data Access（通过依赖注入）
├─ Service → Utility
├─ Data Access → Utility
└─ Utility → Utility

❌ 不允许的依赖
├─ Data Access → Service（违反分层）
├─ Utility → Service（工具不应依赖业务）
└─ Data Access → Data Access（应通过 Service 协调）
```

### 依赖注入示例

**错误方式**（紧耦合）：
```java
// ❌ 直接创建依赖
public class OrderProcessor {
    private OrderRepository repository = new OrderRepository();  // 紧耦合！
    private PriceCalculator calculator = new PriceCalculator();

    public void process(Order order) {
        // ...
    }
}
```

**正确方式**（通过构造注入）：
```java
// ✅ 依赖注入
public class OrderProcessor {
    private final OrderRepository repository;
    private final PriceCalculator calculator;

    // 通过构造函数注入依赖
    public OrderProcessor(OrderRepository repository, PriceCalculator calculator) {
        this.repository = repository;
        this.calculator = calculator;
    }

    public void process(Order order) {
        Money price = calculator.calculate(order);
        order.setPrice(price);
        repository.save(order);
    }
}
```

---

## 组件的粒度

### 过粗的组件

**问题**：
- 职责过多，难以维护
- 难以复用
- 测试困难

**症状**：
- 代码行数 > 1000 行
- 方法数 > 15 个
- 有多个完全独立的职责

**解决**：拆分为多个小组件

### 过细的组件

**问题**：
- 组件过多，难以管理
- 调用链过长，性能下降
- 理解成本高

**症状**：
- 每个方法都在其他组件中
- 单个方法只有几行代码
- 组件之间过多依赖

**解决**：合并相关组件

### 合适的粒度

```
L2 能力 ← 对应 ← 3-5 个 L3 组件
每个 L3 组件：
├─ 代码量：200-500 行
├─ 方法数：3-8 个
├─ 依赖数：1-3 个
└─ 职责数：1 个
```

---

## 组件的可测试性

### 测试金字塔

```
         △△△ E2E 测试 (1-5%)
        △△ 集成测试 (10-20%)
       △ 单元测试 (75-90%)
```

### 可测试的组件设计

```java
// ✅ 易于单元测试的设计
public class OrderValidator {
    // 1. 无外部依赖
    // 2. 纯函数（相同输入 → 相同输出）
    // 3. 清晰的入参和返回值

    public ValidationResult validate(Order order, ValidationRules rules) {
        // 所有依赖通过参数传入
        // 不访问数据库、文件、网络等
        return performValidation(order, rules);
    }
}

// 单元测试
@Test
public void testValidateSuccessful() {
    OrderValidator validator = new OrderValidator();
    Order order = createTestOrder();
    ValidationRules rules = createTestRules();

    ValidationResult result = validator.validate(order, rules);

    assertTrue(result.isValid());
}
```

---

## 组件的版本管理

### 接口版本管理

```java
// v1：第一个版本
public interface OrderValidator {
    ValidationResult validate(Order order);
}

// v2：添加新方法（向后兼容）
public interface OrderValidator {
    ValidationResult validate(Order order);
    ValidationResult validateWithContext(Order order, ValidationContext context);
}

// ❌ v3：删除旧方法（破坏兼容性）
public interface OrderValidator {
    ValidationResult validateWithContext(Order order, ValidationContext context);
}
```

### 版本控制最佳实践

```
发布新版本时：
1. 保持旧接口，标记为 @Deprecated
2. 提供迁移路径和文档
3. 在至少一个版本后才删除旧接口
4. 明确标记版本号变更（major.minor.patch）
```

---

## 组件的文档

### 必需的文档

```
每个组件需要包含：
1. 一句话描述：这个组件做什么
2. 职责说明：明确的职责范围
3. 输入输出：接口定义和数据格式
4. 使用示例：典型用法
5. 依赖说明：依赖哪些组件
6. 性能特性：时间/空间复杂度
7. 已知限制：组件的限制条件
```

### 文档示例

```java
/**
 * 订单价格计算器
 *
 * 职责：根据订单信息和促销规则计算最终价格
 *
 * 输入：
 *   - Order：订单对象（包含订单行、数量、商品价格等）
 *   - DiscountPolicy：促销策略（可选）
 *
 * 输出：
 *   - Money：最终价格，包含基础价格和折扣
 *
 * 使用示例：
 *   Order order = new Order();
 *   order.addLine(item, quantity);
 *   Money price = calculator.calculate(order, discountPolicy);
 *
 * 性能：O(n)，其中 n 是订单行数
 *
 * 已知限制：
 *   - 不支持跨货币转换
 *   - 不支持负数折扣
 */
public class PriceCalculator {
    public Money calculate(Order order, DiscountPolicy policy) {
        // ...
    }
}
```

---

## 关键建议

1. **单一职责**：一个组件只做一件事
2. **清晰接口**：接口设计优于实现
3. **低耦合**：依赖最小化
4. **易测试**：优先设计可测试的组件
5. **合适粒度**：不过大也不过小
6. **良好文档**：让使用者快速理解

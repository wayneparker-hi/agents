# 不变性规则

## 什么是不变性规则？

**定义**：在任何时刻都必须为真的业务约束。聚合存在的目的就是保护这些规则。

## 为什么重要？

### 问题：规则散落在各处

```java
// ❌ 坏的设计：规则散落在应用层
OrderService {
    public void updateOrderAmount(Order order, Money newAmount) {
        if (newAmount.lessThan(ZERO)) {
            throw new InvalidAmountException();
        }
        order.setAmount(newAmount);
    }
}

PaymentService {
    public void processPayment(Order order, Money amount) {
        if (amount.lessThan(ZERO)) {  // 重复检查！
            throw new InvalidAmountException();
        }
        // ...
    }
}
```

**问题**：
- 规则在多个地方重复
- 容易遗漏
- 修改规则时要改多个地方
- 容易出 bug

### 解决：规则在聚合中

```java
// ✅ 好的设计：规则在聚合内
public class Order {
    private Money amount;

    public Order(Money amount) {
        // 构造时就验证
        if (amount.lessThan(ZERO)) {
            throw new InvalidAmountException("订单金额必须 > 0");
        }
        this.amount = amount;
    }

    public void updateAmount(Money newAmount) {
        // 修改时也验证
        if (newAmount.lessThan(ZERO)) {
            throw new InvalidAmountException("订单金额必须 > 0");
        }
        this.amount = newAmount;
    }
}
```

**好处**：
- 规则在一个地方
- 不可能绕过（除非直接修改数据库，那是另一回事）
- 修改规则只需改一个地方

## 识别不变性规则

### 方法 1：从业务流程中识别

问：_"这个业务过程中，什么必须总是真的？"_

**例子**：订单处理

```
Rule 1: Order.totalAmount = sum(OrderLine.amount)
Rule 2: Order.customerId 不能为空
Rule 3: 订单行的数量必须 > 0
Rule 4: 订单的状态转变必须遵循定义的流程
Rule 5: 已发货的订单不能删除
```

### 方法 2：从合法性考虑

问：_"什么情况下数据是非法的？"_

```
Illegal = OrderLine.quantity = -5（负数）
Legal = OrderLine.quantity = 5（正数）

这变成规则：OrderLine.quantity >= 1
```

### 方法 3：从并发冲突识别

问：_"如果两个事务同时修改，会产生什么矛盾？"_

```
事务 1: 将商品 A 的库存从 10 减到 5
事务 2: 同时也从 10 减到 5

结果：库存变成 5（应该是 0）

这说明：库存扣减必须原子化
因此规则：同一个商品的库存修改必须串行化（强一致性）
```

## 不变性规则的类型

### 类型 1：值的约束（Attribute Constraint）

```
Rule: Order.amount > 0
Rule: User.email 是有效的邮箱格式
Rule: Product.stock >= 0
```

### 类型 2：关系约束（Relationship Constraint）

```
Rule: Order.customerId 必须引用有效的 Customer
Rule: OrderLine.orderId 必须指向它所属的 Order
```

### 类型 3：状态机约束（State Machine Constraint）

```
Order 状态转换只能是：
  New → Pending → Confirmed → Shipped → Delivered → Completed
  New → Cancelled （任何时候可以取消新订单）

不允许的转换：
  Completed → New （完成的订单不能回到新建）
  Shipped → Pending （已发货不能回到待确认）
```

### 类型 4：聚合关系约束（Aggregate Constraint）

```
Rule: 同一个订单中，所有 OrderLine 的 orderId 必须相同
Rule: 账户的总额 = 所有交易的累加
```

## 在代码中表达

### 在构造函数中验证

```java
public class Order {
    private final OrderId id;
    private final CustomerId customerId;
    private final Money amount;
    private final List<OrderLine> lines;

    public Order(OrderId id, CustomerId customerId, List<OrderLine> lines) {
        // 验证值约束
        if (customerId == null) {
            throw new BusinessRuleException("顾客 ID 不能为空");
        }

        // 验证聚合关系
        if (lines.isEmpty()) {
            throw new BusinessRuleException("订单必须至少有一项");
        }

        // 验证聚合完整性
        Money calculatedTotal = lines.stream()
            .map(line -> line.getAmount())
            .reduce(Money.ZERO, Money::add);

        this.amount = calculatedTotal;  // 自动计算，不允许外部设置
        this.id = id;
        this.customerId = customerId;
        this.lines = lines;
    }
}
```

### 在命令方法中验证

```java
public class Order {
    private OrderStatus status;

    // 命令：添加一行
    public void addLine(OrderLine line) {
        // 业务规则：只能在 New 状态添加
        if (status != OrderStatus.NEW) {
            throw new BusinessRuleException("已确认的订单不能添加行项");
        }

        lines.add(line);
        recalculateTotal();  // 重新计算总额
    }

    // 命令：确认订单
    public void confirm() {
        // 业务规则：必须有至少一行
        if (lines.isEmpty()) {
            throw new BusinessRuleException("订单必须至少有一项");
        }

        // 业务规则：金额必须 > 0
        if (amount.isLessThan(Money.ZERO)) {
            throw new BusinessRuleException("订单金额必须 > 0");
        }

        // 状态机规则
        if (status != OrderStatus.NEW) {
            throw new BusinessRuleException("只有新订单才能确认");
        }

        this.status = OrderStatus.CONFIRMED;
        // 发布事件
        this.raiseEvent(new OrderConfirmedEvent(this.id));
    }
}
```

## 不变性规则的清单

### 订单聚合

```
□ Order.orderId 唯一且不为空
□ Order.customerId 不为空且必须引用有效 Customer
□ Order.amount = sum(OrderLine.amount)
□ Order.amount > 0
□ Order 必须至少有一个 OrderLine
□ OrderLine.quantity >= 1
□ OrderLine.product 不为空
□ OrderLine.amount = quantity * unitPrice
□ Order 状态转换遵循定义的状态机
□ 已发货的订单不能修改行项
□ 已完成的订单不能取消
```

### 账户聚合

```
□ Account.accountId 唯一且不为空
□ Account.customerId 不为空
□ Account.balance >= 0（假设不允许透支）
□ Account.balance = 初始余额 + sum(入账) - sum(出账)
□ 任何 Transaction 必须正确分类（income/expense）
□ Transaction.amount > 0
□ 冻结的账户不能发生任何交易
```

## 常见错误

| 错误 | 后果 | 修正 |
|------|------|------|
| 规则检查太晚（应用层） | 规则易被绕过 | 在聚合构造和命令中检查 |
| 规则重复检查 | 代码冗余、不一致 | 集中在聚合中 |
| 不变性规则太弱 | 允许非法状态 | 仔细思考并明确所有规则 |
| 不变性规则太强 | 业务操作无法实现 | 考虑是否真的需要强一致性 |
| 没有列举规则 | 新开发者不知道约束 | 明确文档化所有规则 |

## 不变性规则和最终一致性

### 问题：强一致性 vs 最终一致性

```
强一致性：变更后，所有观察者立即看到新状态
最终一致性：变更后，经过一段时间（可能很短）所有观察者才看到新状态
```

### 例子：转账

❌ 错误的做法：在一个聚合中维护两个账户
```
TransferAggregates {
    Account from;
    Account to;
    Money amount;
}
// 问题：聚合太大，冲突多
```

✅ 正确的做法：两个独立聚合，最终一致性
```
FromAccount 聚合：
  - 不变性规则：balance >= 0
  - 命令：withdraw(amount) → WithdrawalCompletedEvent

ToAccount 聚合：
  - 不变性规则：可以接受任何转入
  - 监听：WithdrawalCompletedEvent → deposit(amount)

在短暂时间内，FromAccount 减少了但 ToAccount 还没增加，
但通过事件驱动，最终会一致。
```

---

**核心建议**：
1. 明确列出所有不变性规则
2. 在聚合的构造函数和命令中强制验证
3. 使用异常作为规则违反的反馈
4. 对于需要多个聚合的强一致性需求，考虑是否可以拆分为最终一致性

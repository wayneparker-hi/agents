---
name: domain-modeling
description: 领域建模，包含DDD战术设计、聚合模式、实体值对象设计、不变性规则。在设计领域模型、定义聚合、建模值对象或实现领域驱动设计模式时使用
---

# Domain Modeling

基于 Event Storming 识别的事件和聚合，进一步设计清晰、可维护的**领域模型**。这是 DDD 战术设计的核心，将业务规则明确地表达在代码中。

## When to Use This Skill

- 🏗️ **架构设计**：从应用架构角度，定义核心的业务对象和业务规则
- 💾 **数据建模**：确定哪些数据需要持久化，怎么组织
- 🔄 **流程实现**：将业务流程转化为代码中的命令处理和事件处理
- 🛡️ **业务规则保护**：用领域模型确保业务约束总是被满足
- 📱 **API 设计**：领域模型直接影响 API 的数据结构

## Core Concepts

### 1. 实体（Entity）
**定义**：有唯一身份的对象。两个实体即使所有属性相同，如果 ID 不同就是不同的实体。

**特征**：
- 有唯一的身份标识（ID）
- 可变（属性可以改变）
- 生命周期比较长

**例子**：
- Order（订单）- ID 是订单号
- User（用户）- ID 是用户 ID
- Account（账户）- ID 是账户号

### 2. 值对象（Value Object）
**定义**：没有唯一身份的对象，只代表某个业务概念的值。两个值对象如果所有属性相同，就认为它们相等。

**特征**：
- 没有身份标识
- 不可变（一旦创建就不改变，若要改变就创建新实例）
- 生命周期很短或与实体绑定

**例子**：
- Money（金额）- 有货币和数额
- Address（地址）- 有街道、城市、邮编
- PhoneNumber（电话）- 有国家码、区号、号码

**为什么用值对象？**
- 更清晰的代码：`order.applyDiscount(discount)` 比 `order.discountPercentage = 20` 清楚得多
- 强制不变性：值对象创建后不会改变，减少 bug
- 可复用：Money、Address 等可在多个地方使用
- 更接近业务语言：用 Money 而非 Decimal

### 3. 聚合（Aggregate）
**定义**：一组相关对象的集合，作为数据修改的单位。聚合有一个根实体（聚合根）。

**特征**：
- 有一个聚合根（Aggregate Root）
- 内部有实体和值对象
- 数据一致性边界：聚合内部保证强一致性，聚合外通过事件通信
- 事务边界：一个事务只能修改一个聚合

**例子**：
```
订单聚合
├─ Order（聚合根）
│  ├─ OrderId
│  ├─ CustomerId
│  ├─ OrderStatus
│  └─ OrderLines（集合）
│     ├─ OrderLine1（实体）
│     │  ├─ LineId
│     │  ├─ ProductId
│     │  └─ Quantity（值对象）
│     └─ OrderLine2（实体）
└─ Money（值对象）
   ├─ Currency
   └─ Amount
```

**关键原则**：
1. **最小化聚合**：只包含必须保持强一致性的对象
2. **通过ID引用**：聚合间不要直接持有对象引用，用 ID 代替
3. **通过事件通信**：聚合间的交互通过领域事件，不是直接调用

### 4. 不变性规则（Invariant）
**定义**：在任何时刻都必须为真的业务规则。

**例子**：
- "订单的总金额必须等于订单项金额之和"
- "订单的金额必须大于 0"
- "订单的状态转变必须遵循规定的流程"

**在代码中表达**：
```java
// ❌ 不好：规则散落在应用层
if (order.totalAmount > 1000) {
    // 需要审批
}

// ✅ 好：规则在领域模型中
public class Order {
    public void applyDiscount(Money discount) {
        if (discount.amount >= totalAmount) {
            throw new DomainException("折扣不能超过订单金额");
        }
        // ...
    }
}
```

### 5. 领域服务（Domain Service）
**定义**：当业务逻辑跨越多个聚合或不属于任何单个聚合时，用领域服务。

**例子**：
- TransferService：处理账户转账（跨越两个 Account 聚合）
- PricingService：计算复杂的价格规则（可能涉及多个商品聚合）

**注意**：不要过度使用领域服务，先看能否通过聚合的命令方法实现。

## Step-by-Step Guide

### Step 1: 列出所有业务对象
从 Event Storming 中识别出的聚合出发，列出每个聚合包含的所有对象。

**示例**：
```
订单模块：
- Order（聚合根，实体）
- OrderLine（实体）
- OrderItem（值对象？还是实体？）
- Money（值对象）
- OrderStatus（枚举或值对象）
```

### Step 2: 区分实体和值对象
对每个对象问自己：
- ❓ 这个对象有唯一的身份吗？
- ❓ 这个对象会改变吗？改变后还是"同一个"对象吗？
- ❓ 这个对象会被多个地方引用吗？

### Step 3: 定义聚合根
找出每个聚合的根实体。通常是：
- Event Storming 中标记的对象
- 接收命令的对象
- 产生事件的对象

### Step 4: 定义不变性规则
对每个聚合列出 3-5 个核心的不变性规则。

**模板**：
```
聚合：Order
不变性规则：
1. Order.totalAmount = sum(OrderLine.lineAmount)
2. Order.customerId 不能为空
3. Order 的状态转变必须遵循：新建 → 待支付 → 已支付 → 已发货 → 已完成
4. Order.totalAmount 必须 > 0
5. 订单创建后不能添加优先级超高的订单项
```

### Step 5: 定义命令和事件
每个聚合根应该定义一组命令（它接受的操作）和事件（它产生的事件）。

**命令模板**：
```
Order 聚合的命令：
1. CreateOrder(customerId, items) → OrderCreatedEvent
2. ApplyDiscount(discountId) → DiscountAppliedEvent
3. Cancel() → OrderCancelledEvent
4. ConfirmPayment(paymentId) → PaymentConfirmedEvent
```

### Step 6: 实现聚合
用代码表达上面定义的所有规则。

## Best Practices

1. **模型优先，存储其次**：先设计清晰的领域模型，再考虑如何存储
2. **不变性规则优先**：识别并保护好不变性规则，这是领域模型的精髓
3. **小聚合**：聚合越小越好，这样降低了冲突和锁定
4. **值对象优先**：优先用值对象而非实体，值对象更简单、安全
5. **无贫血模型**：不要设计只有 getter/setter 的模型，应该有行为（命令方法）
6. **持久化透明**：领域模型不应该关心如何持久化，这是存储库的职责

## Common Pitfalls

- **❌ 聚合太大**：包含了不必要的关联对象，导致加载和修改都很重
  → **✅ 解决**：只包含必须保持强一致性的对象

- **❌ 聚合间相互引用**：一个聚合直接持有另一个聚合的对象
  → **✅ 解决**：用 ID 引用，通过事件或服务通信

- **❌ 没有定义不变性规则**：规则散落在应用层
  → **✅ 解决**：明确列出并在聚合内验证

- **❌ 贫血模型**：聚合只有数据，没有业务逻辑
  → **✅ 解决**：将业务逻辑移到聚合的命令方法中

- **❌ 过度建模**：把现实世界的每个细节都建模
  → **✅ 解决**：只建模对架构有影响的东西

## Key Questions

- 哪些对象必须保持强一致性？
- 对象的身份是什么（ID）？
- 哪些是实体，哪些是值对象？
- 核心的不变性规则有哪些？
- 聚合间如何通信？

## References

- `aggregate-design.md` - 聚合设计的详细原则
- `entity-vs-value-object.md` - 实体和值对象的区分方法
- `invariant-rules.md` - 如何识别和表达不变性规则
- `domain-service.md` - 领域服务的设计

## Next Steps

✅ 领域模型设计完成后：
1. 用于**应用架构**设计：聚合直接映射到微服务或应用组件
2. 指导**数据模型**设计：聚合的结构直接影响数据库的表设计
3. 支持**API 设计**：聚合根的命令直接对应 API 端点

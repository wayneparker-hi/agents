# 聚合设计原则

## 什么是聚合？

聚合是 DDD 的核心概念之一。它是一组相关对象的集合，作为**数据修改的最小单位**。

## 关键原则

### 原则 1：最小化聚合

**说法**：聚合应该尽可能小。只包含**必须保持强一致性**的对象。

**为什么**？
- 小聚合更容易维护
- 减少并发冲突
- 降低加载时间
- 支持更好的并发性

**例子**：

❌ **错误**：大订单聚合
```
Order (聚合根)
├── OrderLines (集合)
├── ShippingInfo
├── BillingInfo
├── Payments (集合)
├── Invoices (集合)
├── Reviews (集合)
└── Returns (集合)
```

问题：
- 加载订单时会加载所有相关数据，很重
- 修改时需要锁定很多对象
- 不同的操作有不同的并发要求

✅ **正确**：多个小聚合
```
订单聚合：Order + OrderLines
支付聚合：Payment（独立，通过事件与 Order 通信）
发货聚合：Shipment（独立，通过事件与 Order 通信）
退货聚合：Return（独立，与 Order 分离）
```

优点：
- 每个聚合职责清晰
- 可以独立加载和修改
- 支持并行处理

### 原则 2：通过 ID 引用，不要直接引用

**说法**：聚合间不要直接持有对象引用，只持有 ID。

**为什么**？
- 聚合是独立的数据单位
- 如果直接引用，加载一个聚合会级联加载另一个，违反了最小化原则
- 支持跨越多个数据源（甚至微服务）

**例子**：

❌ **错误**：聚合间直接引用
```java
public class Order {
    private ShippingInfo shippingInfo;  // 直接引用另一个聚合
    private Customer customer;          // 直接引用另一个聚合
}
```

❌ **后果**：
- 加载 Order 时必须加载 ShippingInfo 和 Customer
- 无法独立修改 ShippingInfo
- 如果 ShippingInfo 的数据增加，Order 的加载时间会受影响

✅ **正确**：通过 ID 引用
```java
public class Order {
    private ShippingInfoId shippingInfoId;  // 只持有 ID
    private CustomerId customerId;          // 只持有 ID

    public void assignShipping(ShippingInfoId shippingId) {
        this.shippingInfoId = shippingId;
        // 具体的 shipping 对象需要通过 repository 加载
    }
}
```

✅ **优点**：
- Order 聚合很轻量
- 可以单独加载和修改
- ShippingInfo 可以独立演进

### 原则 3：聚合边界由一致性需求决定

**说法**：如果两个对象必须保持强一致性，它们应该在同一个聚合中。否则应该分离。

**强一致性** = 一旦修改，立即生效，没有延迟。

**例子**：

**Order + OrderLine 在同一聚合**
```
为什么？
- 添加新的 OrderLine 时，Order.totalAmount 必须立即更新
- 这是一个不变性规则：totalAmount = sum(lines.amount)
- 需要原子操作，所以必须在同一事务中
```

**Order vs Payment 分离**
```
为什么分离？
- 支付可能延迟（用户稍后支付）
- 订单创建和支付完成是独立的事件
- 可以采用最终一致性：Order 先创建，支付完成后更新 Order.status
- 通过 PaymentCompletedEvent 通信
```

### 原则 4：一个事务只能修改一个聚合

**说法**：一个业务操作中，如果需要修改多个聚合，要么拆分为多个操作，要么重新考虑聚合边界。

**例子**：

❌ **需要重新设计**：转账操作
```
一个事务同时修改两个账户聚合（FROM_ACCOUNT 和 TO_ACCOUNT）
```

✅ **正确的设计**：使用 Saga 模式
```
操作 1：FromAccount.withdraw() → WithdrawalCompletedEvent
  ↓
操作 2（响应事件）：ToAccount.deposit() → DepositCompletedEvent
  ↓
操作 3（响应事件）：Transfer.markAsCompleted()

如果操作 2 失败，操作 3 会补偿（回滚操作 1）
```

## 实战建议

1. **从不变性规则出发**
   - 列出所有核心的业务规则
   - "X 必须等于 Y"
   - "状态转变必须遵循..."
   - 需要保证这些规则的对象应该在同一聚合

2. **从加载模式考虑**
   - "什么时候需要一起加载？"
   - 如果通常是分开加载的，就应该分开

3. **从修改模式考虑**
   - "什么时候需要一起修改？"
   - 如果修改是独立的，就应该分开

4. **从数据体量考虑**
   - OrderLine 可以很多（几百条）
   - 这时 Order + OrderLines 可能太重
   - 考虑拆分：Order（轻量）+ OrderLines 单独加载

## 常见错误

| 错误 | 原因 | 修正 |
|------|------|------|
| 聚合太大 | 试图在一个聚合中表达所有关系 | 用 ID 引用分解 |
| 聚合太小 | 过度拆分 | 识别真正的一致性需求 |
| 聚合间循环引用 | 设计不清晰 | 明确方向，用事件通信 |
| 聚合违反业务规则 | 没有在聚合中强制规则 | 将规则表达在聚合的方法中 |

---

**关键洞察**：好的聚合设计是围绕**不变性规则**和**数据一致性需求**，而不是"这些对象逻辑相关"。

# 领域模型模板

## 聚合设计模板

### 聚合：[聚合名称]

**聚合根**：[实体名称]

**包含对象**：
```
聚合名称
├─ [实体名]（聚合根）
│  ├─ [属性 1]
│  ├─ [属性 2]
│  └─ [ID]
├─ [实体名]（从实体）
│  └─ [属性]
└─ [值对象名]
   └─ [属性]
```

**示例**：
```
Order 聚合
├─ Order（聚合根，实体）
│  ├─ orderId: OrderId
│  ├─ customerId: CustomerId
│  ├─ status: OrderStatus
│  ├─ createdAt: DateTime
│  └─ lines: List<OrderLine>
├─ OrderLine（从实体）
│  ├─ lineId: LineId
│  ├─ productId: ProductId
│  └─ quantity: Quantity
└─ Money（值对象）
   ├─ amount: BigDecimal
   └─ currency: String
```

**核心不变性规则**：
```
1. Order.orderId 唯一且不为空
2. Order.amount = sum(OrderLine.amount)
3. Order.amount > 0
4. OrderLine.quantity >= 1
5. Order 状态转换遵循定义的流程
```

**命令（聚合根接受的操作）**：
```
- CreateOrder(customerId, lines) → OrderCreatedEvent
- AddLine(productId, quantity) → LineAddedEvent
- UpdateLineQuantity(lineId, newQuantity) → QuantityUpdatedEvent
- ConfirmOrder() → OrderConfirmedEvent
- CancelOrder() → OrderCancelledEvent
```

**事件（聚合根产生的事件）**：
```
- OrderCreatedEvent
  - orderId
  - customerId
  - createdAt

- OrderConfirmedEvent
  - orderId
  - confirmedAt

- OrderCancelledEvent
  - orderId
  - cancelledAt
  - reason
```

---

## 完整的聚合定义示例

### 订单聚合

**聚合根**：`Order`

**职责**：
- 维护订单的生命周期
- 管理订单行项
- 计算订单总额
- 执行订单状态转换

**包含的对象**：
- `Order`（聚合根，实体）
- `OrderLine`（从实体）
- `Money`（值对象 - 金额）
- `OrderStatus`（值对象 - 枚举）

**不变性规则**：
1. Order.amount = sum(lines.amount)
2. Order.amount > 0
3. Order 必须至少有一条 line
4. OrderLine.quantity >= 1
5. 状态转换只能沿着定义的流程进行
6. 已发货的订单不能添加新行

**状态转换图**：
```
      创建
        ↓
    [新建] ← ────────┐
        ↓            │
    [待确认]        取消
        ↓            │
    [已确认]  ────────┘
        ↓
    [已发货]
        ↓
    [已完成]
```

**命令和事件**：
```
命令: CreateOrder
  输入: customerId, items[]
  前置条件: customerId 有效
  后置条件: Order 创建，状态为 New
  事件: OrderCreatedEvent

命令: ConfirmOrder
  输入: -
  前置条件: status == New
  后置条件: status = Confirmed
  事件: OrderConfirmedEvent

命令: CancelOrder
  输入: reason
  前置条件: status ∈ [New, Confirmed]
  后置条件: status = Cancelled
  事件: OrderCancelledEvent
```

---

## 多个聚合的关系

### 场景：订单、支付、发货

```
Order 聚合
├─ orderId
├─ customerId
├─ amount
└─ status

Payment 聚合（独立）
├─ paymentId
├─ orderId (仅保存 ID，不直接引用 Order)
├─ amount
└─ status

Shipment 聚合（独立）
├─ shipmentId
├─ orderId (仅保存 ID，不直接引用 Order)
└─ status
```

**通信方式**：通过领域事件

```
Order.ConfirmOrder() → OrderConfirmedEvent
   ↓
PaymentService 监听 OrderConfirmedEvent
   ↓
Payment.CreatePayment()

Payment.CompletePayment() → PaymentCompletedEvent
   ↓
ShipmentService 监听 PaymentCompletedEvent
   ↓
Shipment.CreateShipment()
```

**关键原则**：
- ✅ Order 和 Payment 是独立聚合
- ✅ 通过 orderId（ID）关联，不直接引用
- ✅ 通过事件通信
- ✅ 各自维护自己的生命周期

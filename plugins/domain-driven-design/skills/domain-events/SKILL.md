---
name: domain-events
description: 领域事件设计与实现。包含事件定义、发布订阅、事件溯源基础、异步通信。在建模重要业务事件、协调聚合间通信、实现最终一致性或构建事件驱动系统时使用
---

# 领域事件

领域事件是 DDD 中表示业务中发生的重要事件的模式。它支持聚合间的通信和系统的演化。

## When to Use This Skill

- 聚合间需要通信
- 需要最终一致性而非强一致性
- 需要审计和追踪系统变化
- 构建事件驱动的系统
- 支持系统的异步处理
- 实现 CQRS 的读写分离

## Core Concepts

### 1. 什么是领域事件

**定义**：在系统中发生的、业务上有重要意义的事件。用过去式命名。

**特征**：
- 业务意义：代表重要的业务发生
- 已发生：用过去式表达
- 不可改变：事件一旦发生就不能改变
- 完整信息：包含理解事件所需的所有信息

**示例**：
```
✓ OrderCreated - 订单已创建
✓ PaymentProcessed - 支付已处理
✓ ShipmentDispatched - 货物已发送
✗ OrderCreating - 订单创建中（应该用命令）
✗ CreateOrder - 命令，不是事件
```

### 2. 事件的结构

```java
public class OrderCreatedEvent implements DomainEvent {
    private final OrderId orderId;           // 事件相关的主体
    private final CustomerId customerId;     // 事件上下文
    private final List<OrderLineItem> items; // 丰富的事件数据
    private final Money totalAmount;
    private final LocalDateTime occurredAt;  // 事件发生时间
    private final String eventId;            // 事件标识
    private final String aggregateType;      // 聚合类型
    private final int version;               // 版本号

    // 事件应该包含所有理解事件所需的数据
    // 不应该强制消费者去查询其他数据
}
```

### 3. 发布和订阅

**发布**：聚合发生变化时发布事件
```java
public class Order {
    public void confirm() {
        // 执行业务逻辑
        this.status = OrderStatus.Confirmed;

        // 发布事件给事件发布器
        DomainEventPublisher.instance()
            .publish(new OrderConfirmedEvent(this.id));
    }
}
```

**订阅**：其他上下文订阅和处理事件
```java
@Component
public class OrderConfirmedEventHandler {
    @EventSubscriber
    public void handleOrderConfirmed(OrderConfirmedEvent event) {
        // 执行相应逻辑（如库存预留）
        inventoryService.reserve(event.getOrderId());
    }
}
```

### 4. 事件存储

事件存储是事件的永久记录：
- 不可变：事件一旦存储不能修改
- 有序：事件按顺序记录
- 可重放：可以从事件重建任何时间点的状态

## Patterns & Practices

### 事件设计最佳实践

1. **丰富事件数据**
   ```java
   // ❌ 贫血事件
   public class OrderCreatedEvent {
       public OrderId orderId;
       // 消费者必须根据 ID 查询完整信息
   }

   // ✓ 富事件
   public class OrderCreatedEvent {
       public OrderId orderId;
       public CustomerId customerId;
       public List<OrderLineItem> items;
       public Money totalAmount;
       public LocalDateTime createdAt;
       // 包含所有必要信息
   }
   ```

2. **事件版本控制**
   ```java
   public class OrderCreatedEvent implements DomainEvent {
       public static final int VERSION = 2; // 版本号

       // 支持多个版本
       public static OrderCreatedEvent fromV1(OrderCreatedEventV1 v1) {
           // 迁移逻辑
       }
   }
   ```

3. **保证幂等性**
   ```java
   // 事件处理应该是幂等的
   // 相同的事件处理多次应该得到相同的结果
   public void handleOrderCreated(OrderCreatedEvent event) {
       // 使用事件 ID 避免重复处理
       if (alreadyProcessed(event.eventId())) return;

       // 处理事件
   }
   ```

### 常见事件模式

1. **最终一致性**
   ```
   Order 聚合 → OrderCreated 事件 → 消息队列 → Inventory 聚合
   (同步)         (异步)              (解耦)
   ```

2. **Saga 模式（分布式事务）**
   ```
   Order → OrderCreated → Payment
           ↓
   Payment → PaymentCompleted → Inventory
           ↓
   Inventory → InventoryReserved → Shipping
   ```

3. **事件溯源**
   ```
   所有状态变化都表示为事件
   当前状态 = 初始状态 + 所有事件的应用
   ```

## Best Practices

1. **事件描述业务，不实现技术**
   - 事件应该代表业务发生
   - 不应该技术细节（不涉及 HTTP、数据库等）

2. **事件包含足够信息**
   - 消费者不应该需要查询其他数据来理解事件
   - 但也不要包含所有可能的信息

3. **异步处理**
   - 事件处理应该异步
   - 不阻塞发布者
   - 支持最终一致性

4. **版本兼容**
   - 新版本事件应该兼容旧版本消费者
   - 添加字段应该有默认值
   - 考虑过渡期

5. **事件监控**
   - 追踪事件发布和处理
   - 监控处理延迟
   - 发现和处理失败的事件

## Resources

- `references/event-sourcing-basics.md` - 事件溯源基础
- `assets/event-schema-template.md` - 事件模式模板
- `assets/event-handler-checklist.md` - 事件处理检查清单

---
name: aggregate-design-principles
description: 聚合设计原则与模式。聚合根、一致性边界、不变量保护、聚合间通信。在设计领域实体及其关系、定义事务边界、保护业务不变量或建模复杂领域逻辑时使用
---

# 聚合设计原则

聚合（Aggregate）是 DDD 战术设计的核心模式。它是一组关联的对象的集合，被视为一个单一的单元来处理数据的更改和一致性。

## When to Use This Skill

- 设计领域模型中的核心对象关系
- 确定事务边界和一致性边界
- 保护业务不变量和规则
- 优化并发和性能
- 设计聚合间的通信和协调
- 实现命令处理和状态转换

## Core Concepts

### 1. 聚合的定义与核心概念

聚合是一个业务概念，它由多个相关的对象组成：

**聚合根（Aggregate Root）**
- 聚合的唯一入口
- 其他对象只能通过根访问
- 负责维护聚合内的不变量
- 外部对象必须持有根的引用，而不是内部对象的引用

**聚合内的对象**
- 实体和值对象
- 只能通过聚合根访问
- 不能被外部直接修改
- 生命周期依附于聚合根

**聚合边界**
- 决定了事务边界
- 一次事务内可以修改一个聚合
- 多个聚合的修改需要通过协调（通常是最终一致性）

### 2. 聚合设计的四个关键原则

#### A. 围绕业务不变量设计

**不变量（Invariant）**：必须始终为真的业务规则。

**设计规则**：
- 识别业务不变量（例如：订单总价 = 所有行项价格之和）
- 将维护这些不变量的对象放在同一聚合中
- 聚合根负责在每次修改时验证不变量

**示例**：Order 聚合
```
不变量：
1. 订单必须至少有一个 OrderLineItem
2. 订单总价 = sum(line_item.price)
3. 已确认订单的内容和价格不能修改
4. 取消的订单不能被重新激活

因此 Order 聚合应该包含：
- Order（根）
- OrderLineItem（内部实体）
- Money（值对象）
- OrderStatus（值对象）
```

#### B. 保持聚合小而专

**聚合大小**：优先考虑小聚合，除非业务规则要求包含更多对象。

**为什么小更好**：
- 并发性更好（锁定范围小）
- 性能更优（加载更快）
- 易于理解和维护
- 易于分布式处理

**分割标准**：
```
大聚合反面模式：
❌ 尽量在一个聚合中放更多内容，最大化代码重用
❌ 为了避免跨聚合调用，把所有相关概念放在一起
❌ 为了数据完整性，将所有关联数据包含进来

小聚合最佳实践：
✓ 只包含维护业务规则所需的对象
✓ 跨聚合通信通过显式接口进行
✓ 多聚合一致性通过最终一致性实现
```

#### C. 通过根限制外部访问

**访问规则**：
- ✓ 外部可以持有并访问聚合根
- ✓ 外部可以调用根的方法
- ✗ 外部不能直接访问或持有根的内部对象
- ✗ 外部不能直接调用内部对象的方法

**实现方式**：
```java
// ❌ 反面示例
public class Order {
    public OrderLineItems getLineItems() {
        return lineItems; // 暴露了内部对象！
    }
}
// 外部代码可以直接修改 Order 的 LineItems，绕过了不变量验证

// ✓ 正确示例
public class Order {
    public void addLineItem(ProductId productId, Quantity qty, Money price) {
        // 验证规则
        if (status == OrderStatus.Confirmed) {
            throw new IllegalStateException("Cannot modify confirmed order");
        }
        // 通过根添加项
        lineItems.add(new OrderLineItem(productId, qty, price));
    }

    public List<OrderLineItemDTO> getLineItems() {
        // 返回 DTO 副本，而非原始对象
        return lineItems.stream()
            .map(item -> new OrderLineItemDTO(item))
            .collect(toList());
    }
}
```

#### D. 通过事件进行聚合间通信

**原则**：聚合间不直接调用或直接修改，通过发布领域事件进行异步通信。

**好处**：
- 聚合解耦
- 支持最终一致性
- 易于添加新的聚合到流程
- 便于审计和追踪

**示例**：订单支付
```
场景：Order 支付完成 → Inventory 需要预留库存

✗ 直接调用方式（紧耦合）：
order.confirmPayment(paymentId);
inventory.reserve(orderId, items); // 直接调用，强耦合

✓ 事件驱动方式（解耦）：
order.confirmPayment(paymentId); // Order 内部发布事件
// PaymentConfirmedEvent published
// Inventory 订阅此事件并执行预留
```

### 3. 聚合识别方法

#### 步骤 1：识别根实体
- 业务流程中的主要参与者
- 具有明确的生命周期
- 通常是业务事件的主体

示例：订单上下文 → Order 是主要的根

#### 步骤 2：识别应该与根一起变化的对象
- 哪些对象的生命周期与根绑定？
- 哪些对象的修改总是与根一起发生？
- 哪些对象的存在必须依赖于根的存在？

示例：OrderLineItem 依赖于 Order，总是一起变化

#### 步骤 3：识别不变量
- 这些对象协作维护什么业务规则？
- 什么规则必须始终为真？
- 什么规则需要事务性保证？

示例：订单总价 = 所有行项价格之和（必须在同一事务中维护）

#### 步骤 4：确定聚合边界
- 将维护不变量的对象放在一起
- 考虑大小和复杂度
- 考虑并发性能

#### 步骤 5：设计对外接口
- 聚合根暴露什么方法给外部？
- 外部如何触发聚合的变化？
- 聚合会发布什么事件？

## Patterns & Practices

### 常见聚合设计模式

**模式 1：单根模式（Simple Aggregate）**
```
Order (Root)
├── OrderLineItem (Entity)
├── OrderLineItem (Entity)
└── Money (Value Object)

特点：根周围包含少量实体
```

**模式 2：递归组合模式（Composite Aggregate）**
```
Category (Root)
├── Category (Child)
│   ├── Category (Child)
│   └── Product (Reference)
└── Product (Reference)

特点：树形结构，但仍然共同维护不变量
```

**模式 3：独立值对象模式（Rich Value Objects）**
```
Order (Root)
├── OrderLineItem (Value Object)
├── OrderLineItem (Value Object)
└── ShippingAddress (Value Object)

特点：使用值对象而非实体减少复杂度
```

### 聚合设计检查清单

- [ ] 是否有明确的根实体？
- [ ] 所有包含的对象都维护同一组不变量吗？
- [ ] 聚合大小是否合理（不超过 5-10 个对象）？
- [ ] 根是否提供了保护内部对象的接口？
- [ ] 跨聚合通信是否通过事件进行？
- [ ] 是否有外部对象持有内部对象的引用？

## Best Practices

1. **优先小聚合**
   - 先设计小聚合
   - 只有业务规则要求时才扩大
   - 定期评审聚合大小

2. **通过行为定义**
   - 围绕业务操作而非数据结构设计
   - 聚合根提供领域语言的方法
   - 避免 getter/setter 暴露内部状态

3. **保护不变量**
   - 在根中验证所有规则
   - 使用值对象强化约束
   - 测试不变量违反情况

4. **异步协调**
   - 使用事件处理跨聚合变化
   - 实现最终一致性
   - 避免分布式事务

5. **聚合版本化**
   - 考虑聚合演化
   - 规划添加新字段或方法的方式
   - 维护 API 稳定性

## Common Pitfalls

1. **God Aggregates**
   - 聚合过大，包含太多不相关的对象
   - 导致并发性能问题
   - 难以测试和维护

2. **数据库驱动设计**
   - 根据数据库关系而非业务规则设计
   - 导致聚合边界不合理

3. **直接引用内部对象**
   - 外部代码绕过聚合根修改内部对象
   - 破坏不变量
   - 导致数据不一致

4. **忽视最终一致性**
   - 尝试同步修改多个聚合
   - 导致分布式事务问题
   - 应该使用事件和 Saga

5. **过度值对象化**
   - 将所有东西都设计为值对象
   - 导致聚合逻辑分散
   - 丢失业务意图

## Resources

- `references/aggregate-patterns.md` - 常见聚合模式详解
- `assets/aggregate-design-checklist.md` - 聚合设计检查清单
- `assets/aggregate-examples.md` - 电商、SaaS 等领域的聚合示例

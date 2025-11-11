---
name: context-mapping-patterns
description: 上下文映射模式。包含协作关系、共享内核、客户供应商、防腐层、开放主机、发布语言。在设计限界上下文之间的关系、规划上下文通信、降低耦合或管理上下文演进时使用
---

# 上下文映射模式

上下文映射定义了限界上下文之间的关系和通信方式。正确的映射模式能够降低耦合，支持独立演化。

## When to Use This Skill

- 设计多个上下文间的协作关系
- 处理跨上下文的数据一致性
- 规划上下文之间的通信
- 处理外部系统集成
- 优化团队间协作
- 管理上下文演化

## Core Concepts

### 八种上下文映射模式

1. **Partnership（合作关系）**
   - 两个上下文对等协作
   - 双向依赖
   - 共同提升，共同失败
   - 适用：紧密合作的两个团队

2. **Shared Kernel（共享内核）**
   - 两个上下文共享一个模型
   - 通过共享代码库维护
   - 最小化共享内容
   - 适用：紧密相关且很难分离

3. **Customer-Supplier（客户-供应商）**
   - 单向依赖：下游依赖上游
   - 上游（供应商）为下游（客户）提供服务
   - 下游可以影响上游的开发优先级
   - 适用：清晰的上下游关系

4. **Conformist（遵奉者）**
   - 下游被迫接受上游的模型
   - 单向依赖
   - 下游没有影响力
   - 适用：依赖第三方系统

5. **Anticorruption Layer（防腐层）**
   - 在两个上下文间创建隔离层
   - 翻译不兼容的模型
   - 保护自己的模型不被污染
   - 适用：与不兼容的系统集成

6. **Open Host Service（开放主机服务）**
   - 上游提供标准的服务接口
   - 允许多个下游连接
   - 版本控制和兼容性
   - 适用：一对多关系

7. **Published Language（发布语言）**
   - 定义公共的交换格式（如 XML、JSON）
   - 供多个消费者使用
   - 版本化和演化
   - 适用：需要独立演化的上下文

8. **Separate Ways（各行其道）**
   - 不集成，各自独立
   - 避免复杂的通信
   - 允许完全不同的技术
   - 适用：低耦合需求

## Patterns & Practices

### 选择映射模式的决策树

```
两个上下文有交互吗？
├─ 否 → Separate Ways
└─ 是 → 它们在同一个团队吗？
   ├─ 是 → Partnership 或 Shared Kernel
   └─ 否 → 有明确的上下游吗？
      ├─ 否 → Partnership
      └─ 是 → 下游有影响力吗？
         ├─ 是 → Customer-Supplier
         └─ 否 → 外部系统吗？
            ├─ 是 → Conformist 或 Anticorruption Layer
            └─ 否 → Customer-Supplier
```

### 防腐层实现

防腐层是最常见的模式。实现策略：

```java
// 外部系统的模型
public class ExternalOrderDTO {
    public String orderId;
    public String customerName;
}

// 我们的模型
public class Order {
    private OrderId id;
    private CustomerId customerId;
}

// 防腐层翻译
public class OrderAdapter {
    public Order toDomain(ExternalOrderDTO dto) {
        OrderId id = OrderId.from(dto.orderId);
        CustomerId customerId = lookupCustomer(dto.customerName);
        return new Order(id, customerId);
    }

    public ExternalOrderDTO toExternal(Order order) {
        return new ExternalOrderDTO(
            order.getId().value(),
            lookupCustomerName(order.getCustomerId())
        );
    }
}
```

## Best Practices

1. **最小化共享内容**
   - 尽量避免 Shared Kernel
   - 如果必须，最小化共享范围

2. **设计稳定的接口**
   - Open Host Service 需要稳定的接口
   - Published Language 需要版本控制

3. **防腐层保护**
   - 与不兼容系统集成时使用防腐层
   - 保护自己的模型

4. **显式记录关系**
   - 清晰地文档化每个映射关系
   - 说明数据流向和职责

## Resources

- `references/integration-patterns.md` - 集成模式详解
- `assets/context-map-template.md` - 上下文地图模板

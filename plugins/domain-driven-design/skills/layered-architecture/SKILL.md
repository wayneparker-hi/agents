---
name: layered-architecture
description: DDD分层架构。包含四层架构、依赖规则、防腐层、层间通信。在设计限界上下文的内部架构、组织代码结构、定义层职责或实现领域模型保护时使用
---

# 分层架构

分层架构是 DDD 中最常用的架构模式。它将系统按照关注点分层，保护领域模型的独立性。

## When to Use This Skill

- 设计单个限界上下文的内部架构
- 组织代码和模块
- 定义层间的依赖关系
- 保护领域逻辑不受技术影响
- 实现关注点分离
- 提高代码可测试性

## Core Concepts

### 1. DDD 的四层架构

```
┌─────────────────────────────────┐
│   User Interface Layer          │  处理用户交互
│   (Presentation/API)            │  - 请求/响应映射
├─────────────────────────────────┤  - 表单验证
│   Application Layer             │
│   (Service/Orchestration)       │  协调业务逻辑
│                                 │  - 用例实现
├─────────────────────────────────┤  - 事务管理
│   Domain Layer                  │
│   (Business Logic)              │  核心业务逻辑
│                                 │  - 聚合、实体、值对象
├─────────────────────────────────┤  - 领域服务、仓储接口
│   Infrastructure Layer          │
│   (Technical Details)           │  技术实现细节
│                                 │  - 数据库、消息、外部API
└─────────────────────────────────┘
```

### 2. 四层详解

#### A. 用户接口层（User Interface Layer）

**职责**：
- 处理用户请求和交互
- 将用户输入转换为应用命令
- 将应用结果转换为用户友好的格式
- 验证用户输入的格式（不是业务规则）

**包含内容**：
- REST/GraphQL API
- Web Controller
- Web Form
- CLI
- 请求/响应 DTO

**依赖**：依赖应用层

```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {
    private OrderService orderService;

    @PostMapping
    public OrderResponse createOrder(@RequestBody CreateOrderRequest request) {
        // 格式验证
        Order order = orderService.createOrder(
            request.getCustomerId(),
            request.getItems()
        );
        return new OrderResponse(order);
    }
}
```

#### B. 应用层（Application Layer）

**职责**：
- 协调业务逻辑执行
- 实现用例和业务流程
- 事务管理和跨聚合协调
- 将领域逻辑与应用逻辑分离

**包含内容**：
- 应用服务（Application Service）
- 命令处理器
- 查询处理器
- DTO 和映射

**特点**：
- 不包含业务规则（规则在领域层）
- 不直接访问基础设施（通过接口）
- 协调、不做决策

```java
@Service
public class CreateOrderService {
    private OrderRepository orderRepository;
    private InventoryService inventoryService;
    private OrderFactory orderFactory;

    @Transactional
    public Order createOrder(CustomerId customerId, List<OrderLineItem> items) {
        // 1. 创建聚合（业务规则在聚合中）
        Order order = orderFactory.createOrder(customerId, items);

        // 2. 预留库存（跨聚合操作）
        inventoryService.reserve(order);

        // 3. 保存聚合
        orderRepository.save(order);

        // 4. 返回结果给应用调用者
        return order;
    }
}
```

#### C. 领域层（Domain Layer）

**职责**：
- 表达业务概念
- 保护业务不变量
- 实现业务规则
- 提供领域语言的API

**包含内容**：
- 聚合（Aggregate）
- 实体（Entity）
- 值对象（Value Object）
- 领域服务（Domain Service）
- 仓储接口（Repository Interface）

**特点**：
- 不依赖应用层或基础设施层
- 不包含框架代码
- 100% 可测试（无需 Mock 基础设施）

```java
public class Order {
    private OrderId id;
    private CustomerId customerId;
    private List<OrderLineItem> lineItems;
    private OrderStatus status;

    // 业务规则在这里
    public void confirmPayment(PaymentId paymentId) {
        if (status != OrderStatus.Pending) {
            throw new InvalidOrderStatusException(
                "Only pending orders can be confirmed"
            );
        }
        this.status = OrderStatus.Confirmed;
        // 发布事件
        DomainEventPublisher.publish(
            new OrderConfirmedEvent(this.id, paymentId)
        );
    }

    // 仓储接口（依赖倒置）
    public interface Repository {
        void save(Order order);
        Optional<Order> findById(OrderId id);
    }
}
```

#### D. 基础设施层（Infrastructure Layer）

**职责**：
- 实现持久化
- 与外部系统集成
- 提供通信机制
- 完成特定技术工作

**包含内容**：
- ORM/数据库访问
- 仓储实现
- 外部 API 客户端
- 消息队列集成
- 缓存实现

**特点**：
- 对上层隐藏技术细节
- 通过接口被应用层使用
- 可替换（改技术栈时）

```java
@Repository
public class JpaOrderRepository implements Order.Repository {
    private OrderJpaRepository jpaRepository;

    @Override
    public void save(Order order) {
        OrderEntity entity = OrderMapper.toEntity(order);
        jpaRepository.save(entity);
    }

    @Override
    public Optional<Order> findById(OrderId id) {
        return jpaRepository.findById(id.value())
            .map(OrderMapper::toDomain);
    }
}
```

### 3. 依赖规则

```
依赖方向：上层依赖下层，不能反向依赖

UI Layer
   ↓
Application Layer
   ↓
Domain Layer
   ↓
Infrastructure Layer

正确：
✓ UI 层可以调用应用层
✓ 应用层可以调用领域层
✓ 基础设施层实现领域接口

错误：
✗ 领域层依赖应用层
✗ 基础设施层调用 UI 层
✗ 领域对象直接访问数据库
```

### 4. 防腐层（Anti-Corruption Layer）

在与外部系统集成时，用防腐层保护领域模型：

```java
// 外部系统的模型
public class ExternalPaymentGateway {
    public PaymentResult process(String orderId, String amount) { ... }
}

// 防腐层翻译
@Component
public class PaymentGatewayAdapter {
    private ExternalPaymentGateway gateway;

    public void processPayment(OrderId orderId, Money amount) {
        try {
            PaymentResult result = gateway.process(
                orderId.value(),
                amount.amount().toString()
            );
            // 翻译为领域概念
            if (result.isSuccess()) {
                DomainEventPublisher.publish(
                    new PaymentProcessedEvent(orderId)
                );
            }
        } catch (ExternalException e) {
            // 处理外部异常，不让其污染领域
            DomainEventPublisher.publish(
                new PaymentFailedEvent(orderId, e.getMessage())
            );
        }
    }
}
```

## Patterns & Practices

### 层间通信最佳实践

1. **使用 DTO（Data Transfer Object）**
   ```java
   // ❌ 不要在层间传递领域对象
   public Order getOrder(OrderId id) {
       return orderRepository.findById(id); // 暴露了领域对象
   }

   // ✓ 使用 DTO
   public OrderDTO getOrder(OrderId id) {
       Order order = orderRepository.findById(id);
       return OrderMapper.toDTO(order);
   }
   ```

2. **单一责任**
   - 应用层协调，不做业务决策
   - 领域层做决策，不做应用编排
   - 基础设施层处理技术，不涉及业务逻辑

3. **测试金字塔**
   ```
   端到端测试（少）
   ↑
   集成测试（中）
   ↑
   单元测试（多）

   领域层应该有最多的单元测试
   应用层有中等数量集成测试
   UI 层有端到端测试
   ```

## Best Practices

1. **保护领域层独立**
   - 领域层不依赖应用或基础设施
   - 依赖倒置：应用依赖领域定义的接口

2. **应用层很薄**
   - 应用层是协调层，不是逻辑层
   - 业务规则属于领域层
   - 应用服务通常只有 5-10 行代码

3. **清晰的层边界**
   - 每层有明确的职责
   - 跨层通信通过定义的接口
   - 避免跳层访问

4. **技术决策隔离**
   - 技术细节在基础设施层
   - 上层代码不知道使用的是什么数据库
   - 便于更换技术栈

## Resources

- `references/architecture-evolution.md` - 架构演进路径
- `assets/layer-checklist.md` - 分层检查清单
- `assets/layer-responsibilities.md` - 每层职责详解

---
name: entity-value-object-patterns
description: 实体与值对象模式。包含标识设计、不变性、领域行为、对象等值性。在设计领域对象、确定对象标识策略、建模不可变概念或决定实体和值对象时使用
---

# 实体与值对象模式

实体和值对象是 DDD 的两个基本概念，它们代表了不同类型的领域概念。选择正确的类型对模型的清晰性和可维护性至关重要。

## When to Use This Skill

- 设计领域对象时，需要判断是实体还是值对象
- 决定对象的标识策略
- 实现对象相等性和哈希
- 设计不可变值对象
- 建模复杂的业务概念
- 优化内存和数据库使用

## Core Concepts

### 1. 实体（Entity）

**定义**：具有唯一标识、生命周期和可变状态的对象。

**关键特征**：
- **唯一标识**：每个实体有一个不改变的标识（如 OrderId）
- **生命周期**：实体会经历创建、修改、删除
- **可变性**：实体的属性可以改变
- **身份相等**：两个实体相等当且仅当标识相同

**实体的标识策略**：
```
1. 通用身份（Universal Identity）
   - UUID / GUID：数据库无关
   - 优点：分布式安全，性能好
   - 缺点：对用户不友好

2. 领域有意义的身份（Domain Meaningful Identity）
   - 订单号、客户代码：业务上有意义
   - 优点：可读性高，业务语言
   - 缺点：需要实现唯一性校验

3. 组合身份（Composite Identity）
   - 多个字段组合（租户ID + 用户ID）
   - 优点：精细化控制
   - 缺点：复杂性增加
```

**实体设计示例**：
```java
public class Order {
    private OrderId id;           // 标识
    private CustomerId customerId;
    private List<OrderLineItem> lineItems;
    private OrderStatus status;   // 可变状态

    // 标识相等
    @Override
    public boolean equals(Object other) {
        if (this == other) return true;
        if (!(other instanceof Order)) return false;
        return id.equals(((Order)other).id);
    }

    @Override
    public int hashCode() {
        return id.hashCode();
    }
}
```

### 2. 值对象（Value Object）

**定义**：没有唯一标识、不可变、由属性值定义的对象。

**关键特征**：
- **无身份**：没有标识，不需要跟踪生命周期
- **不可变**：创建后不能改变
- **等值性**：两个值对象如果属性值相同就相等
- **自描述**：通过属性值完整描述自己

**值对象的优势**：
- 易于理解和使用
- 安全地共享和传递
- 易于测试
- 性能更好（没有身份跟踪开销）

**值对象设计示例**：
```java
public class Money {
    private final BigDecimal amount;
    private final Currency currency;

    // 不可变
    public Money(BigDecimal amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }

    // 等值性：值相同即相等
    @Override
    public boolean equals(Object other) {
        if (!(other instanceof Money)) return false;
        Money that = (Money)other;
        return this.amount.equals(that.amount) &&
               this.currency.equals(that.currency);
    }

    @Override
    public int hashCode() {
        return Objects.hash(amount, currency);
    }

    // 业务行为
    public Money add(Money other) {
        if (!currency.equals(other.currency)) {
            throw new CurrencyMismatchException();
        }
        return new Money(amount.add(other.amount), currency);
    }
}
```

### 3. 实体 vs 值对象决策树

```
这个概念需要被跟踪吗？
├─ 是 → 它有唯一的身份吗？
│  ├─ 是 → 它会改变吗？
│  │  ├─ 是 → Entity（可变实体）
│  │  └─ 否 → 考虑值对象或实体
│  └─ 否 → 可能不应该单独建模
└─ 否 → 它可以安全地被共享和复制吗？
   ├─ 是 → Value Object（如果能保证不可变）
   └─ 否 → 可能需要实体来控制生命周期
```

### 4. 值对象的自验证

值对象应该在构造时验证自己的有效性：

```java
public class Quantity {
    private final int amount;

    public Quantity(int amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Quantity must be positive");
        }
        this.amount = amount;
    }

    // 量的加法
    public Quantity add(Quantity other) {
        return new Quantity(this.amount + other.amount);
    }
}
```

## Patterns & Practices

### 常见实体模式

1. **富实体模式**
   - 实体包含业务逻辑
   - 提供领域语言的方法
   - 自我保护不变量

2. **贫血实体模式**
   - ❌ 只有数据，没有行为
   - 逻辑分散在服务中
   - 难以维护不变量

### 常见值对象模式

1. **自描述值对象**
   ```java
   public class Email {
       private final String value;

       public Email(String value) {
           if (!isValid(value)) throw new InvalidEmailException();
           this.value = value;
       }

       private static boolean isValid(String email) {
           return email.matches("^[\\w.+-]+@[\\w.-]+\\.\\w+$");
       }
   }
   ```

2. **值对象集合**
   ```java
   public class Tags {
       private final Set<String> tags;

       public Tags(Set<String> tags) {
           this.tags = Collections.unmodifiableSet(tags);
       }

       public Tags add(String tag) {
           Set<String> newTags = new HashSet<>(tags);
           newTags.add(tag);
           return new Tags(newTags);
       }
   }
   ```

## Best Practices

1. **优先使用值对象**
   - 更简单、更安全
   - 只在需要跟踪生命周期时用实体

2. **保护值对象的不可变性**
   - 返回副本而非原始集合
   - 防止外部修改

3. **在构造时验证**
   - 值对象应该在创建时就是有效的
   - 避免之后通过 setter 破坏有效性

4. **使用有意义的标识**
   - 选择对业务有意义的标识
   - 考虑如何生成和验证

5. **避免实体引用歧义**
   - 清晰地标记可变性
   - 避免意外修改

## Common Pitfalls

1. **把所有东西都设计为实体**
   - 导致过度复杂
   - 性能问题

2. **值对象中的可变集合**
   ```java
   // ❌ 错误
   public class Order {
       public List<Item> getItems() {
           return items; // 可被外部修改！
       }
   }

   // ✓ 正确
   public class Order {
       public List<Item> getItems() {
           return Collections.unmodifiableList(items);
       }
   }
   ```

3. **忽视标识的含义**
   - 导致相同语义的实体被认为不同
   - 应该清晰定义标识的业务含义

4. **在值对象中使用 getter/setter**
   - 暗示可变性
   - 应该用业务方法替代

5. **跨限界上下文共享值对象**
   - 创建不必要的耦合
   - 每个上下文应该有自己的定义

## Resources

- `references/identity-design-guide.md` - 标识设计详解
- `assets/value-object-examples.md` - 常见值对象示例
- `assets/entity-value-object-checklist.md` - 设计检查清单

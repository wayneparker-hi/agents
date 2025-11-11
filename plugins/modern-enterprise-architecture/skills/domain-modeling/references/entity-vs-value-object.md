# 实体 vs 值对象

## 核心区别

| 特征 | 实体 | 值对象 |
|------|------|--------|
| **身份** | 有唯一身份（ID） | 无身份，由属性定义 |
| **相等性** | 同 ID 才相等 | 所有属性相等就相等 |
| **可变性** | 可变 | 不可变 |
| **生命周期** | 较长，独立存在 | 较短，依附于实体 |
| **持久化** | 单独表 / 文档 | 嵌入父实体 |
| **引用** | 通过 ID 引用 | 直接传递（复制） |

## 详细讲解

### 实体（Entity）

**定义**：有唯一身份，代表某个具体的业务对象，会改变但保持同一性。

**特征**：
1. **有身份标识**
   - ID（自然 ID 或代理 ID）
   - 两个实体即使所有属性相同，ID 不同就是不同的实体

2. **可变**
   - 属性可以改变
   - 改变后仍是同一个实体

3. **较长的生命周期**
   - 存在于系统中较久
   - 有开始和结束的明确时间

4. **需要身份跟踪**
   - 系统需要知道"这是同一个对象"
   - 常见的 ACID 数据库操作

**例子**：

```
Customer 实体
├─ CustomerId: "CUST-12345"  ← 身份
├─ Name: "张三"
├─ Email: "zhangsan@example.com"
├─ CreatedAt: 2024-01-01
└─ UpdatedAt: 2024-11-10

修改后：
├─ CustomerId: "CUST-12345"  ← 还是同一个 Customer
├─ Name: "张三"              ← 可能改变
├─ Email: "newemail@example.com"  ← 改变了
└─ UpdatedAt: 2024-11-11
```

**为什么重要**：
- 系统需要跟踪"这个客户"
- 两个名叫"张三"的客户，如果 ID 不同就是不同的人
- 客户的信息会变化，但他/她仍是同一个人

### 值对象（Value Object）

**定义**：没有身份，只代表某个值或概念，不可变，两个值对象如果属性相同就认为相等。

**特征**：
1. **无身份**
   - 不需要 ID
   - 由属性完全定义

2. **不可变**
   - 一旦创建就不改变
   - 要"改变"就创建新实例

3. **较短生命周期**
   - 依附于实体
   - 实体删除时一起删除

4. **按值比较**
   - 两个值对象属性相同就相等
   - 不需要跟踪"身份"

**例子**：

```
Money 值对象
├─ Amount: 100.00
└─ Currency: "CNY"

修改"金额"：
不是修改原对象，而是创建新对象
original = Money(100, "CNY")
discounted = Money(80, "CNY")  ← 新对象

Money(100, "CNY") == Money(100, "CNY")  ← true（属性相同就相等）
```

**为什么重要**：
- 不需要跟踪身份，更简单
- 不可变意味着更安全（没有并发问题）
- 可以在多个地方复用而不用担心副作用

## 实战识别

### 问题 1：这是实体还是值对象？

**例子：Address（地址）**

```
如果你系统中需要：
❌ "地址 ID"
❌ 跟踪"同一个地址的变化"
❌ 单独的地址表/集合

→ 这是值对象
```

**例子：Order（订单）**

```
如果你系统中需要：
✅ "订单 ID"（OrderNo: "SO-2024-001"）
✅ 跟踪"这个订单"从创建到完成的全生命周期
✅ 订单的状态变化

→ 这是实体
```

### 问题 2：如果有重复的值对象怎么办？

```
Customer {
  homeAddress: Money(80.50, "CNY")  ← 金额
  workAddress: Money(80.50, "CNY")  ← 同样的金额
}

这是正常的！值对象就是这样的。
两个 Money(80.50, "CNY") 是"相等的"，即使是不同的对象。
```

### 问题 3：值对象能包含实体吗？

```
❌ 不行。值对象不应该包含实体。
如果包含了实体，那说明它不是真正的值对象。

✅ 正确的做法：值对象可以包含其他值对象
```

## 在代码中表达

### 值对象的实现原则

```java
// ✅ 好的值对象实现
public class Money {
    private final BigDecimal amount;
    private final String currency;

    public Money(BigDecimal amount, String currency) {
        this.amount = amount;
        this.currency = currency;
    }

    // 不可变：没有 setter

    // 按值比较
    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Money)) return false;
        Money other = (Money) obj;
        return amount.equals(other.amount) &&
               currency.equals(other.currency);
    }

    @Override
    public int hashCode() {
        return Objects.hash(amount, currency);
    }

    // 操作返回新对象
    public Money add(Money other) {
        return new Money(
            this.amount.add(other.amount),
            this.currency
        );
    }
}
```

### 实体的实现原则

```java
// ✅ 好的实体实现
public class Customer {
    private final CustomerId id;  // 身份
    private String name;          // 可变
    private Email email;          // 值对象
    private Address address;      // 值对象

    public Customer(CustomerId id, String name, Email email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    // 身份相等
    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Customer)) return false;
        return this.id.equals(((Customer) obj).id);
    }

    @Override
    public int hashCode() {
        return id.hashCode();
    }

    // 可以改变属性
    public void updateEmail(Email newEmail) {
        this.email = newEmail;
    }
}
```

## 常见误区

### 误区 1：把所有东西都设计成值对象

❌ 错误：Address 定义为值对象，但系统需要查询"某个地址的所有客户"

✅ 修正：Address 应该是实体（或者单独的地址表）

### 误区 2：为值对象增加 ID

```java
❌ 错误：
public class Money {
    private UUID id;  // 值对象不需要 ID！
    private BigDecimal amount;
    private String currency;
}

✅ 正确：
public class Money {
    private BigDecimal amount;
    private String currency;
}
```

### 误区 3：值对象可变

```java
❌ 错误：
public class Money {
    private BigDecimal amount;

    public void setAmount(BigDecimal newAmount) {  // 错误！值对象应不可变
        this.amount = newAmount;
    }
}

✅ 正确：
public class Money {
    private final BigDecimal amount;

    public Money add(BigDecimal delta) {
        return new Money(this.amount.add(delta));
    }
}
```

## 为什么这很重要？

### 代码清晰性
```
Money price;  // 一眼知道这是个值，不会变
Customer customer;  // 知道这是个实体，有生命周期
```

### 性能
```
值对象可以被拷贝、缓存、比较，没有引用问题
实体需要通过 ID 引用，避免加载过多数据
```

### 并发安全
```
值对象不可变，完全线程安全
实体有状态，需要同步或 Immutable 模式
```

---

**核心建议**：优先使用值对象。只在需要身份和生命周期跟踪时才用实体。

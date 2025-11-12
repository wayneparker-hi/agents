---
name: plantuml-best-practices
description: PlantUML最佳实践技能，指导如何使用PlantUML绘制4+1架构图。包括各类图的标准模板、颜色约定、排版规范、自动化生成和版本控制最佳实践
---

# PlantUML最佳实践技能 (PlantUML Best Practices Skill)

## 概述

PlantUML是文本驱动的UML图表生成工具，使用简单的文本语法生成高质量的架构和设计图表。本技能指南提供PlantUML在4+1架构视图中的最佳实践。

## 核心概念

### 1. PlantUML的优势

**相比传统visio等工具**：
```
✅ 优势：
  - 文本驱动，便于版本控制
  - 易于团队协作和代码审查
  - 快速迭代和修改
  - 自动布局，减少调整
  - 支持多种输出格式（PNG, SVG, PDF）
  - 开源免费

❌ 劣势：
  - 学习曲线（但并不陡峭）
  - 对复杂图表的手动控制有限
  - 生成的图表样式有限
```

### 2. 支持的图表类型

**4+1架构中常用的图表**：
```
场景视图：用例图 (Use Case Diagram)
逻辑视图：类图 (Class Diagram)
开发视图：组件图 (Component Diagram)、包图 (Package Diagram)
进程视图：序列图 (Sequence Diagram)、活动图 (Activity Diagram)
物理视图：部署图 (Deployment Diagram)
其他：状态图 (State Diagram)、时序图 (Timing Diagram)
```

### 3. PlantUML的基础语法

**基本结构**：
```plantuml
@startuml
title 图表标题
' 这是注释

' 图表内容
participant A
participant B
A -> B: 消息

@enduml
```

## 设计流程

### Step 1：选择合适的图表类型

根据要表达的内容选择合适的图表类型。

**选择指南**：
```
要表达          图表类型         关键要素
─────────────────────────────────────────────
用例关系        用例图          参与者、用例、系统边界
类结构关系      类图            类、属性、方法、关系
模块结构        组件图          组件、依赖、接口
包组织          包图            包、嵌套、依赖
对象交互        序列图          参与者、消息、时序
流程步骤        活动图          活动、分支、循环、同步
运行时行为      状态图          状态、转换、条件
部署物理        部署图          节点、部署单位、连接
```

### Step 2：定义基本元素

根据所选图表类型，定义基本元素。

**元素定义示例**：

**用例图**：
```plantuml
@startuml
actor User as U
actor Admin as A
usecase "查询订单" as UC1
usecase "创建订单" as UC2
usecase "管理用户" as UC3

U --> UC1
U --> UC2
A --> UC3
@enduml
```

**类图**：
```plantuml
@startuml
class Order {
  - orderId: Long
  - user: User
  - items: List<OrderItem>
  + addItem(product, quantity)
  + confirmPayment(payment)
}

class User {
  - userId: Long
  - name: String
  - orders: List<Order>
}

class OrderItem {
  - itemId: Long
  - product: Product
  - quantity: int
}

Order "1" *-- "1..*" OrderItem
Order "N" --> "1" User
@enduml
```

**序列图**：
```plantuml
@startuml
actor Consumer
participant OrderService
participant Database
participant PaymentGateway

Consumer -> OrderService: 创建订单请求
activate OrderService
OrderService -> Database: 保存订单
activate Database
Database --> OrderService: 返回订单ID
deactivate Database
OrderService -> PaymentGateway: 获取支付链接
activate PaymentGateway
PaymentGateway --> OrderService: 支付URL
deactivate PaymentGateway
OrderService --> Consumer: 返回订单和支付信息
deactivate OrderService
@enduml
```

### Step 3：定义关系和连接

使用合适的连接符号表达不同的关系。

**关系符号对照表**：
```
UML关系         PlantUML语法      含义
────────────────────────────────────────
关联            -->              引用关系
聚合            o--              整体与部分
组合            *--              强依赖部分
继承            --|>             类继承
实现            ..|>             接口实现
依赖            ..>              依赖关系

多重性标注
────────────────────────────────────────
一对一          "1" --> "1"
一对多          "1" --> "*" 或 "1" --> "0..*"
多对多          "*" --> "*"
```

**关系定义示例**：
```plantuml
@startuml
class Order {
  - items: List<OrderItem>
}

class OrderItem {
  - product: Product
}

class Product {}

class User {}

' 组合关系：Order包含OrderItem
Order "1" *-- "1..*" OrderItem : contains

' 关联关系：OrderItem引用Product
OrderItem "N" --> "1" Product : references

' 关联关系：Order属于User
Order "N" --> "1" User : belongs to
@enduml
```

### Step 4：添加详细信息和注释

丰富图表的信息，提高可读性。

**颜色和样式**：
```plantuml
@startuml
skinparam backgroundColor #FEFEFE
skinparam classBackgroundColor #FFE4E1
skinparam classBorderColor #FF6347
skinparam classArrowColor #FF6347

class Order {
  {field} - orderId: Long
  {method} + addItem()
  {method} + confirmPayment()
}

note right of Order
  订单是核心业务对象
  负责订单生命周期管理
end note
@enduml
```

**分层和分组**：
```plantuml
@startuml
package "API Layer" {
  class OrderController
  class PaymentController
}

package "Service Layer" {
  class OrderService
  class PaymentService
}

package "Domain Layer" {
  class Order
  class Payment
}

OrderController --> OrderService
OrderService --> Order
@enduml
```

### Step 5：验证和优化

检查图表的完整性和可读性。

**验证清单**：
```
□ 所有关键元素都包含了？
□ 关系是否准确？
□ 是否清晰易读？
□ 颜色搭配是否合理？
□ 是否包含必要的注释？
□ 是否符合命名规范？
```

## 5+1架构中的PlantUML应用

### 场景视图 - 用例图

**标准模板**：
```plantuml
@startuml 电商平台用例图
skinparam backgroundColor #FEFEFE
title 电商平台 - 场景视图（用例图）

' 定义参与者
actor Consumer as C
actor Merchant as M
actor Admin as A
actor PaymentGateway as PG

' 定义系统边界
rectangle "电商系统" {
  ' 消费者用例
  usecase "浏览商品" as Browse
  usecase "搜索商品" as Search
  usecase "查看详情" as ViewDetail
  usecase "添加购物车" as AddCart
  usecase "下单" as Checkout
  usecase "支付" as Payment
  usecase "查询订单" as QueryOrder

  ' 商家用例
  usecase "发布商品" as Publish
  usecase "查看销售" as ViewSales

  ' 管理员用例
  usecase "管理用户" as ManageUser
  usecase "审核商品" as AuditProduct

  ' 关系定义
  C --> Browse
  C --> Search
  C --> ViewDetail
  C --> AddCart
  C --> Checkout
  C --> Payment
  C --> QueryOrder

  M --> Publish
  M --> ViewSales

  A --> ManageUser
  A --> AuditProduct

  ' 用例间的关系
  Checkout .> AddCart : include
  Checkout .> Payment : include
  ViewSales ..> Publish : references

  ' 外部系统
  Payment --> PG
}

@enduml
```

**最佳实践**：
```
✅ DO:
  - 用名词-动词结构命名用例
  - 用清晰的系统边界
  - 明确show参与者和系统的交互

❌ DON'T:
  - 过多的用例（>20个）
  - 复杂的用例间依赖
  - 模糊的用例名称
```

### 逻辑视图 - 类图

**标准模板**：
```plantuml
@startuml 电商平台类图
!include <C4/C4_Component>

skinparam backgroundColor #FEFEFE
skinparam classBackgroundColor #E1F5FE
skinparam classBorderColor #01579B

title 电商平台 - 逻辑视图（类图）

' 定义包
package "Domain Model" {
  class User {
    - userId: Long
    - name: String
    - email: String
    + register()
    + login()
  }

  class Order {
    - orderId: Long
    - user: User
    - items: List<OrderItem>
    - status: OrderStatus
    + addItem(product, quantity)
    + confirm()
  }

  class OrderItem {
    - itemId: Long
    - product: Product
    - quantity: int
  }

  class Product {
    - productId: Long
    - name: String
    - price: Money
  }

  class Money {
    - amount: BigDecimal
    - currency: String
  }

  enum OrderStatus {
    NEW
    PAID
    SHIPPED
    DELIVERED
  }
}

' 定义关系
User "1" --> "0..*" Order : places
Order "1" *-- "1..*" OrderItem : contains
OrderItem "N" --> "1" Product : includes
Order "1" --> "1" Money : total amount
Order "1" --> "1" OrderStatus : has status

' 添加注释
note right of Order
  订单是核心业务聚合
  包含订单项和状态信息
end note

@enduml
```

**最佳实践**：
```
✅ DO:
  - 显示关键属性和方法
  - 清晰标注多重性
  - 使用适当的关系符号
  - 分包显示

❌ DON'T:
  - 显示所有细节（类太复杂）
  - 混乱的关系（过多的连线）
  - 不标注多重性
```

### 开发视图 - 组件图

**标准模板**：
```plantuml
@startuml 电商平台开发视图
!include <C4/C4_Component>

skinparam backgroundColor #FEFEFE
SHOW_PERSON_OUTLINE()

title 电商平台 - 开发视图（组件图）

' 表现层
package "API Layer" {
  component [OrderController]
  component [PaymentController]
}

' 应用服务层
package "Service Layer" {
  component [OrderService]
  component [PaymentService]
}

' 业务逻辑层
package "Domain Layer" {
  component [Order]
  component [Payment]
}

' 数据访问层
package "Repository Layer" {
  component [OrderRepository]
  component [PaymentRepository]
}

' 基础设施层
package "Infrastructure Layer" {
  component [Database]
  component [Cache]
  component [MessageQueue]
}

' 依赖关系
[OrderController] --> [OrderService] : uses
[OrderService] --> [Order] : uses
[OrderService] --> [OrderRepository] : uses
[OrderRepository] --> [Database] : queries
[OrderRepository] --> [Cache] : caches

[PaymentController] --> [PaymentService] : uses
[PaymentService] --> [PaymentRepository] : uses
[PaymentService] --> [MessageQueue] : sends

@enduml
```

### 进程视图 - 序列图

**标准模板 - 下单场景**：
```plantuml
@startuml 电商平台下单流程
title 电商平台 - 进程视图（序列图）

actor Consumer
participant Web as W
participant OrderService as OS
participant InventoryService as IS
participant Database as DB
participant PaymentGateway as PG

autonumber

Consumer -> W: 点击下单
activate W

par 并行查询
  W -> OS: 创建订单请求
  activate OS
  OS -> IS: 查询库存
  activate IS
  IS -> DB: 查询库存数量
  activate DB
  DB --> IS: 返回数量
  deactivate DB
  IS --> OS: 库存充足
  deactivate IS
end

OS -> DB: 保存订单记录
activate DB
DB --> OS: 订单ID
deactivate DB

OS -> IS: 预扣库存
activate IS
IS -> DB: 更新库存
activate DB
DB --> IS: 成功
deactivate DB
IS --> OS: 预扣成功
deactivate IS

OS -> PG: 请求支付链接
activate PG
PG --> OS: 支付URL
deactivate PG

OS --> W: 返回订单号和支付URL
deactivate OS

W --> Consumer: 显示支付页面
deactivate W

Consumer -> PG: 进行支付
@enduml
```

### 进程视图 - 活动图

**标准模板 - 支付流程**：
```plantuml
@startuml 电商平台支付流程
title 电商平台 - 进程视图（活动图）

start
:显示支付方式选择页面;

if (用户选择支付方式?) then (支付宝)
  :调用支付宝API;
  :用户扫码授权;
  :支付宝返回结果;
elseif (微信支付) then
  :调用微信API;
  :用户微信授权;
  :微信返回结果;
elseif (银行卡) then
  :显示卡号输入;
  :用户输入卡号和验证码;
  :银行返回结果;
else (其他)
  :显示错误信息;
  stop
endif

if (支付成功?) then (是)
  :记录支付日志;
  :更新订单状态为已支付;
  :发送成功通知;
  :显示成功页面;
else (否)
  :记录失败日志;
  :显示失败页面;
  :提示重试;
endif

stop
@enduml
```

### 物理视图 - 部署图

**标准模板**：
```plantuml
@startuml 电商平台部署视图
!include <C4/C4_Deployment>

title 电商平台 - 物理视图（部署图）

Deployment_Node(internet, "互联网", "") {
}

Deployment_Node(aws_region, "AWS Region", "us-east-1") {
  Deployment_Node(dmz, "DMZ 公网区", "Public Subnet") {
    Deployment_Node(cdn, "CDN", "CloudFront") {
    }
    Deployment_Node(lb, "负载均衡器", "ALB") {
    }
  }

  Deployment_Node(app_zone, "应用服务器区", "Private Subnet") {
    Deployment_Node(app1, "应用服务器1", "EC2 t3.large") {
      Component(app_comp1, "Order Service", "Java App")
    }
    Deployment_Node(app2, "应用服务器2", "EC2 t3.large") {
      Component(app_comp2, "Order Service", "Java App")
    }
    Deployment_Node(app3, "应用服务器N", "EC2 t3.large") {
      Component(app_comp3, "Order Service", "Java App")
    }
  }

  Deployment_Node(data_zone, "数据存储区", "Private Subnet") {
    Deployment_Node(db_primary, "主数据库", "RDS Multi-AZ") {
      Component(db, "PostgreSQL", "Database")
    }
    Deployment_Node(cache, "缓存", "ElastiCache") {
      Component(redis, "Redis", "Cache")
    }
    Deployment_Node(mq, "消息队列", "RabbitMQ") {
      Component(rabbit, "RabbitMQ", "Message Queue")
    }
  }
}

Deployment_Node(backup_region, "备份区", "us-west-2") {
  Deployment_Node(backup_db, "从数据库", "RDS Read Replica") {
    Component(backup, "PostgreSQL", "Database")
  }
}

' 连接关系
internet --> cdn: HTTPS
cdn --> lb: Content
lb --> app1
lb --> app2
lb --> app3

app_comp1 --> db: SQL
app_comp2 --> db: SQL
app_comp3 --> db: SQL

app_comp1 --> redis: Cache
app_comp1 --> rabbit: Async

db --> backup: Replication

@enduml
```

## 常用的PlantUML指令和技巧

### 1. 自定义样式

```plantuml
@startuml
' 全局样式
skinparam backgroundColor #FEFEFE
skinparam classBackgroundColor #E1F5FE
skinparam classBorderColor #0277BD
skinparam classArrowColor #0277BD
skinparam fontSize 14
skinparam defaultFontName Arial

' 特定元素样式
class HighPriority {
  {background:#FF6347}
}

class LowPriority {
  {background:#90EE90}
}

@enduml
```

### 2. 隐藏和显示元素

```plantuml
@startuml
hide empty attributes
hide empty methods
hide circle
show package
@enduml
```

### 3. 排版和布局控制

```plantuml
@startuml
' 设置方向
!direction left to right

' 设置布局
top to bottom direction
' left to right direction

' 增加间距
skinparam padding 10

@enduml
```

### 4. 图例和注释

```plantuml
@startuml
note right of ElementA
  这是一个多行注释
  可以跨越多行
  用来解释设计
end note

legend right
  |color #FFAAAA | 高优先级 |
  |color #AAFFAA | 低优先级 |
end legend
@enduml
```

## PlantUML应用检查清单

使用PlantUML生成图表时，检查以下内容：

- [ ] **语法正确** - 图表能正常生成，没有语法错误
- [ ] **元素完整** - 所有关键元素都包含了
- [ ] **关系准确** - 关系和多重性标注正确
- [ ] **易于阅读** - 排版和颜色搭配合理
- [ ] **注释清晰** - 关键元素都有适当的注释
- [ ] **命名规范** - 所有元素使用清晰的名称
- [ ] **版本控制** - 图表源代码可版本控制
- [ ] **符合规范** - 遵循UML规范和4+1架构标准
- [ ] **导出格式** - 支持多种输出格式（PNG, SVG, PDF）
- [ ] **可维护性** - 修改和维护图表容易

## 常见问题

**Q: PlantUML支持中文吗？**
A: 支持，但需要配置合适的字体。建议使用SimSun或Microsoft YaHei字体。

**Q: 如何导出高质量的图表？**
A: 使用SVG格式获得矢量图，可以缩放而不失质量。PNG适合直接显示。

**Q: 如何在文档中集成PlantUML？**
A: 可以使用插件集成到Markdown、Confluence、GitLab等工具中。

**Q: 大图表如何处理？**
A: 分解成多个较小的图表，或使用!define进行模块化。

## 参考资源

- 官方网站：http://plantuml.com/
- 在线编辑器：http://www.plantuml.com/plantuml/uml/
- 官方文档：http://plantuml.com/guide

---

PlantUML的优势在于它的文本驱动特性，使得图表可以和代码一起进行版本控制和代码审查，这对于架构设计的长期维护至关重要。

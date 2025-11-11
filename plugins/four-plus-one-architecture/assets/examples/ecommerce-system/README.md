# 电商系统 (E-Commerce System) - 4+1架构设计示例

## 目录
- [系统概述](#系统概述)
- [场景视图 (Scenario View)](#场景视图)
- [逻辑视图 (Logical View)](#逻辑视图)
- [开发视图 (Development View)](#开发视图)
- [进程视图 (Process View)](#进程视图)
- [物理视图 (Physical View)](#物理视图)
- [非功能需求](#非功能需求)
- [架构决策记录](#架构决策记录)

---

## 系统概述

这是一个典型的B2C电商系统，支持消费者浏览购买商品、商家发布管理商品、管理员审核管理的完整电商平台。

### 核心功能模块

| 模块 | 说明 | 关键业务 |
|------|------|--------|
| **用户管理** | 消费者/商家/管理员账户管理 | 注册、登录、个人中心 |
| **商品管理** | 商品信息、库存、分类 | 发布、搜索、展示、库存扣减 |
| **购物车** | 临时购物记录 | 加入、删除、计价、结算 |
| **订单管理** | 订单全生命周期 | 创建、支付、发货、退货 |
| **支付管理** | 多渠道支付 | 支付宝、微信支付、银行转账 |
| **物流配送** | 发货和追踪 | 面单生成、物流查询、签收 |

### 业务规模指标

| 指标 | 数值 |
|------|------|
| **日均用户** | 50万+ |
| **日均订单** | 1000万+ |
| **高峰QPS** | 5000+ |
| **目标可用性** | 99.95% |
| **平均响应时间** | <500ms (P95: <1000ms) |

---

## 场景视图

**文件**: `1-场景视图.plantuml`

### 视图说明

场景视图描述系统的功能需求，展示参与者与系统的交互。

### 主要参与者

| 参与者 | 说明 | 主要用例 |
|--------|------|--------|
| **消费者** | 最终用户，采购商品 | 浏览、搜索、购物、支付、查询订单、退货 |
| **商家** | 商品发布者 | 发布商品、管理库存、查看订单、设置运费 |
| **管理员** | 系统管理者 | 管理商家、审核商品、审核订单、数据报表 |
| **支付网关** | 外部系统 | 支付处理 |
| **物流系统** | 外部系统 | 物流对接 |

### 核心用例

#### 消费者用例 (8个)
1. **UC1: 浏览商品** - 浏览商品列表
2. **UC2: 搜索商品** - 按关键词搜索 (includes UC1)
3. **UC3: 查看商品详情** - 查看商品详细信息
4. **UC4: 加入购物车** - 添加商品到购物车
5. **UC5: 生成订单** - 从购物车创建订单 (includes UC4)
6. **UC6: 支付订单** - 发起支付流程 (includes UC5)
7. **UC7: 确认支付** - 支付网关回调确认
8. **UC9: 查询订单** - 查看订单状态和物流信息
9. **UC10: 退货退款** (extends UC9)

#### 商家用例 (4个)
1. **UC11: 发布商品** - 商家发布新商品
2. **UC12: 管理库存** (includes UC11) - 更新库存
3. **UC13: 查看订单** - 查看相关订单
4. **UC14: 设置运费** (includes UC12) - 配置运费策略

#### 管理员用例 (4个)
1. **UC15: 管理商家** - 审核和管理商家账户
2. **UC16: 管理商品** (extends UC15) - 审核商品发布
3. **UC17: 审核订单** (extends UC15) - 处理问题订单
4. **UC18: 查看数据报表** - 生成运营报表

---

## 逻辑视图

**文件**: `2-逻辑视图.plantuml`

### 视图说明

逻辑视图展示系统的核心数据模型和业务对象。使用DDD（领域驱动设计）方法组织。

### 核心领域对象

#### 用户域 (User Domain)

```
User (用户基类)
├── Customer (消费者) - 包含收货地址、优选设置、评论历史
├── Merchant (商家) - 包含店铺信息、银行账户、商品列表
└── Admin (管理员) - 包含权限配置
```

**关键类**:
- `User`: 基础用户信息 (userId, username, email, password)
- `Customer`: 扩展消费者特定属性 (addresses, preferences)
- `Merchant`: 扩展商家特定属性 (storeName, bankInfo, rating)
- `Address`: 收货地址信息
- `Preferences`: 用户偏好设置

#### 商品域 (Product Domain)

```
Product (商品)
├── Category (分类) - 树形结构
├── Review (评论) - 消费者评价
└── Merchant (发布者)
```

**关键类**:
- `Product`: 商品核心属性 (name, price, stock, rating, merchant)
- `Category`: 分层分类 (categoryId, name, parent, children)
- `Review`: 商品评论和评分

#### 购物车域 (Cart Domain)

```
Cart (购物车)
└── CartItem[] (购物车项)
    └── Product (关联商品)
```

**关键类**:
- `Cart`: 消费者购物车 (cartId, customer, items, totalPrice)
- `CartItem`: 购物车中的单个商品 (product, quantity, price)

#### 订单域 (Order Domain)

```
Order (订单)
├── OrderItem[] (订单项)
├── Payment (支付)
└── Shipment (发货)
```

**关键类**:
- `Order`: 订单主体 (orderId, customer, items, status, totalPrice)
  - Status: PENDING → PAID → SHIPPED → DELIVERED
- `OrderItem`: 订单中的商品 (product, quantity, unitPrice)
- `Payment`: 支付记录 (amount, paymentMethod, status, transactionId)
- `Shipment`: 发货记录 (logistics, trackingNumber, status)

#### 支付域 (Payment Domain)

```
Payment (支付单)
├── PaymentMethod (支付方式)
│   ├── Alipay (支付宝)
│   ├── WeChat Pay (微信支付)
│   ├── Credit Card (信用卡)
│   └── Bank Transfer (银行转账)
└── PaymentStatus (支付状态)
    ├── PENDING
    ├── SUCCESS
    ├── FAILED
    └── REFUNDED
```

#### 物流域 (Logistics Domain)

```
Shipment (发货单)
├── LogisticsProvider (物流商)
├── TrackingInfo (跟踪信息)
└── ShipmentStatus (发货状态)
```

### 关键设计原则

1. **Money值对象**: 使用 `Money` 类封装金额，避免直接使用 `BigDecimal`
2. **枚举用于状态**: `OrderStatus`, `PaymentStatus`, `ShipmentStatus` 等
3. **聚合根**: `Order`, `Payment`, `Cart` 作为各自的聚合根
4. **关系**: 遵循DDD原则，避免循环依赖

---

## 开发视图

**文件**: `3-开发视图.plantuml`

### 视图说明

开发视图展示系统的代码组织结构，包含包结构、分层架构、模块依赖。

### 包结构

系统采用**按功能领域组织，按分层细分**的结构：

```
com.example.ecommerce/
├── user/                          # 用户模块
│   ├── domain/                    # 领域层
│   │   ├── User.java
│   │   └── UserService.java
│   ├── application/               # 应用层
│   │   ├── RegisterService.java
│   │   └── LoginService.java
│   ├── infrastructure/            # 基础设施层
│   │   └── UserDao.java
│   └── api/                       # API层
│       └── UserController.java
│
├── product/                       # 商品模块
│   ├── domain/
│   │   ├── Product.java
│   │   ├── Category.java
│   │   └── ProductService.java
│   ├── application/
│   │   └── ProductManager.java
│   ├── infrastructure/
│   │   ├── ProductDao.java
│   │   └── SearchIndex.java
│   └── api/
│       └── ProductController.java
│
├── order/                         # 订单模块
│   ├── domain/
│   │   ├── Order.java
│   │   └── OrderService.java
│   ├── application/
│   │   ├── PlaceOrderService.java
│   │   └── OrderQueryService.java
│   ├── infrastructure/
│   │   ├── OrderDao.java
│   │   └── OrderEventPublisher.java
│   └── api/
│       └── OrderController.java
│
├── payment/                       # 支付模块
│   ├── domain/
│   │   ├── Payment.java
│   │   └── PaymentService.java
│   ├── application/
│   │   ├── PaymentManager.java
│   │   └── RefundService.java
│   ├── infrastructure/
│   │   ├── PaymentGatewayClient.java
│   │   └── PaymentDao.java
│   └── api/
│       ├── PaymentController.java
│       └── CallbackHandler.java
│
├── shipment/                      # 发货模块
│   ├── domain/
│   │   ├── Shipment.java
│   │   └── LogisticsProvider.java
│   ├── application/
│   │   ├── ShipManager.java
│   │   └── TrackingService.java
│   ├── infrastructure/
│   │   ├── ShipmentDao.java
│   │   └── LogisticsApiClient.java
│   └── api/
│       └── ShipmentController.java
│
├── cart/                          # 购物车模块
│   ├── domain/
│   │   ├── Cart.java
│   │   └── CartService.java
│   ├── application/
│   │   └── CartManager.java
│   ├── infrastructure/
│   │   ├── CartCache.java
│   │   └── CartDao.java
│   └── api/
│       └── CartController.java
│
└── common/                        # 共享模块
    ├── domain/
    │   ├── BaseEntity.java
    │   ├── Money.java
    │   └── Address.java
    ├── infrastructure/
    │   ├── BaseRepository.java
    │   ├── EventBus.java
    │   ├── CacheManager.java
    │   └── Logger.java
    └── api/
        ├── ErrorHandler.java
        ├── RequestInterceptor.java
        └── ResponseFormatter.java
```

### 分层架构 (4层)

#### 1. **API层 (Presentation Layer)**
- 职责: HTTP请求处理、参数校验、响应序列化
- 组件: `*Controller`, `RequestInterceptor`, `ErrorHandler`
- 依赖: Application Service

#### 2. **应用层 (Application Layer)**
- 职责: 业务流程协调、事务管理、服务编排
- 组件: `*Service` (应用服务)
- 依赖: Domain Service, Infrastructure

#### 3. **业务逻辑层 (Domain Layer)**
- 职责: 核心业务规则、域对象、业务逻辑
- 组件: Entity, Value Object, Domain Service
- 依赖: 无其他层依赖 (纯业务逻辑)

#### 4. **数据访问层 (Infrastructure Layer)**
- 职责: 数据库访问、缓存、外部系统集成
- 组件: `*Dao`, `*Repository`, 外部API客户端
- 依赖: Domain (Repository接口)

### 模块依赖关系

```
OrderController
    ↓
PlaceOrderService (应用层协调)
    ↓
OrderService (业务逻辑)
    ↓
OrderRepository (数据访问)
    ↓
Database

PlaceOrderService 还需要:
- InventoryService (库存扣减)
- OrderEventPublisher (事件发布)
- CartService (购物车查询)
```

### 关键设计原则

1. **高内聚**: 同一领域的相关类放在同一包中
2. **低耦合**: 模块之间通过接口通信，避免直接依赖
3. **单向依赖**: 上层依赖下层，不允许逆向依赖
4. **分层清晰**: 每层只依赖下层，不能跨层

---

## 进程视图

**文件**: `4-进程视图.plantuml`

### 视图说明

进程视图展示系统在运行时的行为，特别是关键业务流程和并发特性。

### 关键场景

#### 场景1: 下单流程 (消费者下单)

```
时序:
1. 消费者提交订单请求
2. API网关接收请求，转发给购物车服务
3. 购物车服务查询购物车项
4. 订单服务创建订单
5. [并行执行]
   - 库存服务扣减库存
   - 订单服务保存订单记录
6. 发布订单创建事件到消息队列
7. 返回订单ID给消费者

性能指标:
- P50 latency: ~200ms
- P95 latency: ~500ms
- P99 latency: ~1000ms
```

**关键设计**:
- 库存扣减和订单保存并行执行，加快处理速度
- 事件发布异步进行，不阻塞主流程

#### 场景2: 支付流程

```
时序:
1. 消费者发起支付请求
2. 支付服务创建支付单
3. 返回支付链接，消费者跳转到支付网关
4. 支付网关处理支付（异步）
5. 支付网关回调系统
6. 支付服务更新订单为已支付
7. 发布支付成功事件
8. 触发发货流程

性能指标:
- 支付请求: ~100ms (同步部分)
- 后续处理: 异步，不影响用户体验
```

#### 场景3: 发货和物流跟踪

```
时序:
1. 商家获取待发货订单列表
2. 商家确认发货
3. 系统调用物流API生成面单
4. 保存发货信息
5. 发布发货事件，通知消费者
6. 消费者可实时查询物流进度
7. 系统定期同步物流信息

并发处理:
- 多个商家并发发货不互相影响
- 物流信息查询不涉及数据库写入，可高度并发
```

### 并发分析

#### 高峰场景计算

```
日均订单: 1000万
工作时间: 8小时
平均QPS: 1000万 / (8 * 3600) ≈ 347 QPS
高峰系数: 5倍
高峰QPS: 347 * 5 = 1735 QPS

假设单个API实例容量: 500 QPS
所需实例数: 1735 / 500 ≈ 4个
加上故障转移冗余(+1): 5个实例
```

#### 并发模型

- **Web容器**: Tomcat (线程池 200+)
- **数据库连接**: HikariCP (30个连接/实例)
- **缓存连接**: Jedis连接池 (10个)
- **消息队列**: RabbitMQ连接 (5个)

### 性能优化策略

1. **缓存策略**:
   - 商品信息: Redis TTL=1小时
   - 分类信息: Redis TTL=6小时
   - 用户收货地址: Redis TTL=永久

2. **异步处理**:
   - 订单创建事件 → 消息队列
   - 支付回调 → 消息队列
   - 物流更新 → 消息队列

3. **数据库优化**:
   - 订单表: 按商家分片
   - 索引: 订单状态、创建时间、消费者ID
   - 读写分离: 从库处理查询

---

## 物理视图

**文件**: `5-物理视图.plantuml`

### 视图说明

物理视图展示系统的部署架构、网络拓扑、高可用方案。

### 部署拓扑

#### 互联网层
- **CDN**: Aliyun CDN - 加速静态资源分发 (图片/CSS/JS)

#### DMZ区 (互联网边界)
- **负载均衡**: SLB (Server Load Balancer)
  - 3个实例 (2个主活 + 1个备份)
  - 支持HTTPS/HTTP流量
  - 自动故障转移
  - 跨可用区部署

#### 应用区 (业务应用)

**API服务集群**:
- 5个ECS实例 (ecs.c6.xlarge, 4GB内存, 2 vCPU)
- 每个实例支持 ~500 QPS
- 总容量: 2500 QPS (正常)，高峰时3000+ QPS (可弹性扩容)
- 部署: Docker容器化，支持快速扩缩容

**缓存集群** (Redis):
- 3个实例: 1主2从
- 容量: 64GB (单实例)
- 持久化: RDB + AOF
- 自动故障转移

**消息队列** (RabbitMQ):
- 3个节点集群
- 支持事件驱动、异步处理
- 消息持久化
- 队列镜像备份

#### 数据区 (数据存储)

**数据库集群** (MySQL):
- **主库**: 1个实例
  - 规格: db.r6i.4xlarge (16GB内存, 500GB存储)
  - 存储: SSD 高速存储

- **从库**: 2个实例 (读专用)
  - 用于读流量分散
  - 主从同步延迟 < 1秒

- **高可用配置**:
  - 主从复制（异步）
  - 自动故障转移 (MHA 或 MGR)
  - 数据备份：日备份 + 增量备份
  - RTO (Recovery Time Objective): 5分钟
  - RPO (Recovery Point Objective): 1分钟

**搜索引擎** (Elasticsearch):
- 3个节点：2个数据节点 + 1个协调节点
- 用途: 商品搜索、日志聚合
- 索引副本: 1个备份

**备份存储** (OSS):
- 对象存储服务
- 跨地域备份冗余
- 备份策略: 每天全量备份 + 增量备份

#### 管理区 (运维管理)

**监控系统**:
- Prometheus: 采集系统指标 (CPU, 内存, 磁盘, 网络)
- Grafana: 数据可视化和仪表板
- Alertmanager: 告警管理和通知

**日志系统** (ELK Stack):
- Elasticsearch: 日志存储和搜索
- Logstash: 日志采集和处理
- Kibana: 日志分析和展示

**CI/CD**:
- GitLab CI/CD: 自动化部署流程
- Docker Registry: 镜像仓库

### 网络拓扑

```
Internet
    ↓ HTTPS/HTTP:443
[CDN] ← 静态资源
    ↓
[SLB] ← 负载均衡
    ↓ 内网:8080
[API Services × 5] ← 应用集群
    ├─→ [Redis Cluster] ← 缓存 (端口6379)
    ├─→ [RabbitMQ Cluster] ← 消息队列 (端口5672)
    ├─→ [MySQL] ← 数据库 (端口3306)
    │    ├─ Master (写)
    │    └─ Slaves × 2 (读)
    ├─→ [Elasticsearch] ← 搜索引擎 (端口9200)
    ├─→ [Payment Gateway] ← 支付 (HTTPS)
    ├─→ [Logistics API] ← 物流 (HTTPS)
    └─→ [SMS Service] ← 短信 (SDK)
```

### 防火墙规则

| 方向 | 源 | 目标 | 端口 | 协议 | 说明 |
|------|-----|------|------|------|------|
| Inbound | Internet | SLB | 443 | HTTPS | 用户请求 |
| Inbound | Internet | SLB | 80 | HTTP | HTTP重定向 |
| Inbound | SLB | API Services | 8080 | TCP | 内网转发 |
| Outbound | API Services | Payment Gateway | 443 | HTTPS | 支付集成 |
| Outbound | API Services | Logistics API | 443 | HTTPS | 物流集成 |
| Inbound | Internal | Database | 3306 | TCP | 数据库访问 |
| Inbound | API Services | Redis | 6379 | TCP | 缓存访问 |
| Inbound | API Services | RabbitMQ | 5672 | TCP | 消息队列 |

### 容量规划

#### CPU/内存配置

| 组件 | 实例数 | 规格 | 内存/个 | 总内存 |
|------|--------|------|--------|--------|
| API Services | 5 | 4GB | 4GB | 20GB |
| Redis | 3 | 64GB | 64GB | 192GB |
| MySQL Master | 1 | 16GB | 16GB | 16GB |
| MySQL Slave | 2 | 16GB | 16GB | 32GB |
| Elasticsearch | 3 | 16GB | 16GB | 48GB |
| **总计** | - | - | - | **308GB** |

#### 成本估算 (月度)

| 组件 | 数量 | 单价 | 小计 |
|------|------|------|------|
| API服务器 (ECS) | 5 | ¥150 | ¥750 |
| 数据库 (RDS) | 3 | ¥500 | ¥1500 |
| 缓存 (Redis) | 3 | ¥200 | ¥600 |
| 消息队列 | 3 | ¥100 | ¥300 |
| CDN/OSS | 1 | ¥200 | ¥200 |
| 负载均衡 | 1 | ¥100 | ¥100 |
| 监控/日志 | 1 | ¥200 | ¥200 |
| **总计** | - | - | **¥3650/月** |

---

## 非功能需求

### 性能需求

| 指标 | 目标 | 说明 |
|------|------|------|
| **响应时间 P50** | <200ms | 用户感知良好 |
| **响应时间 P95** | <500ms | 大多数用户满意 |
| **响应时间 P99** | <1000ms | 极端情况可接受 |
| **吞吐量 (QPS)** | 5000+ | 高峰时段 |
| **缓存命中率** | >80% | 减少DB压力 |

### 可用性需求

| 指标 | 目标 | 说明 |
|------|------|------|
| **系统可用率** | 99.95% | 年度不超过2.2小时宕机 |
| **数据库可用率** | 99.99% | 主从切换 RTO < 5分钟 |
| **RTO** | 5分钟 | 故障恢复时间 |
| **RPO** | 1分钟 | 数据丢失风险 |

### 可扩展性需求

| 维度 | 方案 |
|------|------|
| **水平扩展** | 应用无状态，支持弹性扩缩容 |
| **数据库扩展** | 按商家分片，支持库表分片 |
| **缓存扩展** | Redis集群，支持在线扩容 |
| **消息队列** | RabbitMQ集群，支持在线扩展 |

### 安全需求

| 需求 | 实现 |
|------|------|
| **身份认证** | JWT Token + 刷新机制 |
| **授权管理** | RBAC (Role-Based Access Control) |
| **数据加密** | 传输: TLS/SSL, 存储: 敏感字段加密 |
| **SQL注入防护** | 参数化查询，ORM框架 |
| **XSS防护** | 输入检验，HTML转义 |
| **CSRF防护** | Token验证 |
| **DDoS防护** | SLB + WAF (Web应用防火墙) |

### 可维护性需求

| 需求 | 实现 |
|------|------|
| **代码规范** | CheckStyle, SonarQube |
| **日志记录** | ELK Stack, 结构化日志 |
| **监控告警** | Prometheus + Grafana |
| **链路追踪** | Jaeger (分布式追踪) |
| **文档完整** | JavaDoc, API文档 (Swagger) |

---

## 架构决策记录

### ADR-001: 采用DDD分层架构

**决策**: 在开发视图中采用DDD (Domain-Driven Design) 风格的分层架构

**原因**:
1. 业务复杂度高，DDD能更好地建模复杂业务
2. 减少业务层与技术层耦合
3. 便于单元测试和独立部署
4. 团队对DDD有经验

**方案**:
- 4层架构: API → Application → Domain → Infrastructure
- 按功能领域（user, product, order等）纵向分割
- 每个领域内进行横向分层

**权衡**:
- 优点: 业务清晰、易于维护、易于扩展
- 缺点: 代码层级多、学习曲线陡峭

---

### ADR-002: 采用MySQL + Redis缓存架构

**决策**: 使用MySQL作为主数据库，Redis作为缓存层

**原因**:
1. MySQL成熟稳定，支持ACID事务
2. 团队熟悉MySQL运维
3. 阿里云RDS MySQL提供完整的高可用方案
4. Redis能显著提升读性能

**方案**:
- MySQL 主从复制，读写分离
- Redis 3节点集群，支持自动故障转移
- 缓存更新: 旁路模式 (Cache-Aside)

**权衡**:
- 优点: 成熟、稳定、易于运维
- 缺点: 缓存一致性难度大、可能有缓存穿透问题

---

### ADR-003: 采用事件驱动架构

**决策**: 关键业务流程（订单、支付、发货）采用事件驱动模式

**原因**:
1. 解耦各个业务模块
2. 支持异步处理，提升响应时间
3. 便于扩展新的业务流程
4. 便于调试和审计

**方案**:
- 使用RabbitMQ作为消息中间件
- 订单创建、支付成功、发货等事件发布到MQ
- 相关模块订阅事件并异步处理

**权衡**:
- 优点: 高度解耦、易于扩展
- 缺点: 消息可靠性难度大、消息顺序保证复杂

---

### ADR-004: 采用API网关模式

**决策**: 在SLB和各个服务之间添加API网关

**原因**:
1. 统一入口，便于认证、授权、日志记录
2. 请求路由和负载均衡
3. 限流和熔断保护
4. API版本管理

**方案**:
- 使用Kong或自研轻量级网关
- 统一处理跨切面关注点（认证、日志等）

**权衡**:
- 优点: 统一控制、易于管理
- 缺点: 网关成为性能瓶颈、故障点

---

### ADR-005: 采用按商家分片策略

**决策**: 订单数据按商家分片，避免单表过大

**原因**:
1. 单表数据量可能达到100亿+，查询性能下降
2. 按商家分片是自然的业务维度
3. 便于扩展到多数据库甚至地理分布

**方案**:
- 选择 merchant_id 作为分片键
- 使用分片中间件 (如 ShardingSphere) 或应用层实现
- 跨商家查询通过聚合实现

**权衡**:
- 优点: 解决大表问题、易于扩展
- 缺点: 分布式事务复杂、跨分片查询困难

---

### ADR-006: 采用异步支付回调处理

**决策**: 支付网关回调通过消息队列异步处理

**原因**:
1. 支付网关回调必须快速响应（超时会重试）
2. 后续流程（订单确认、发货）比较耗时
3. 消息队列提供可靠的异步处理

**方案**:
- 支付回调立即返回 HTTP 200
- 事件发送到消息队列
- 消费者订阅事件，异步更新订单状态

**权衡**:
- 优点: 响应快、系统解耦
- 缺点: 消息可靠性要求高、调试复杂

---

## 总结

这个电商系统的4+1架构设计展示了：

1. **场景视图**: 清晰的用例划分，明确的参与者和交互
2. **逻辑视图**: DDD领域建模，清晰的业务对象和关系
3. **开发视图**: 按功能领域和分层组织的代码结构
4. **进程视图**: 关键业务流程和并发特性分析
5. **物理视图**: 完整的部署架构、高可用方案和容量规划

同时通过架构决策记录，说明了每个重要设计的理由和权衡，便于未来的维护和演进。

---

**文档版本**: v1.0
**最后更新**: 2024年
**维护人**: 架构团队

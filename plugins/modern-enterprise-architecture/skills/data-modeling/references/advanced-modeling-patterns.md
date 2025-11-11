# 高级数据建模模式与实战案例

## 概述

本文档展示了在实际项目中常见的高级数据建模场景和解决方案。

---

## 案例 1：电商平台的多维度数据建模

### 业务场景

```
电商平台需要支持：
  ├─ 订单管理
  ├─ 库存追踪
  ├─ 销售分析
  ├─ 用户行为分析
  └─ 推荐系统
```

### 核心数据模型

```
维度表：
  ├─ dim_customer（客户维度）
  │  ├─ customer_id (SK)
  │  ├─ customer_no (BK)
  │  ├─ customer_name
  │  ├─ customer_type
  │  ├─ first_purchase_date
  │  ├─ lifetime_value
  │  ├─ vip_level
  │  └─ (SCD Type 2: vip_level 变化追踪)
  │
  ├─ dim_product（产品维度）
  │  ├─ product_id (SK)
  │  ├─ product_code (BK)
  │  ├─ product_name
  │  ├─ category_id
  │  ├─ brand_id
  │  ├─ supplier_id
  │  ├─ list_price
  │  ├─ cost_price
  │  └─ (SCD Type 2: 价格变化追踪)
  │
  ├─ dim_date（日期维度）
  │  ├─ date_id (SK)
  │  ├─ calendar_date (BK)
  │  ├─ day_of_week
  │  ├─ month
  │  ├─ quarter
  │  ├─ year
  │  ├─ is_holiday
  │  └─ is_promotion_day
  │
  ├─ dim_location（地点维度）
  │  ├─ location_id (SK)
  │  ├─ location_name (BK)
  │  ├─ province
  │  ├─ city
  │  ├─ region
  │  └─ warehouse_code
  │
  └─ dim_channel（销售渠道维度）
     ├─ channel_id (SK)
     ├─ channel_name
     ├─ channel_type (官网/天猫/京东/线下)
     └─ commission_rate

事实表：
  ├─ fact_order（订单事实）
  │  ├─ order_id (BK)
  │  ├─ customer_id (FK → dim_customer)
  │  ├─ order_date_id (FK → dim_date)
  │  ├─ total_amount
  │  ├─ discount_amount
  │  ├─ final_amount
  │  ├─ order_status
  │  └─ (粒度：一行一个订单)
  │
  ├─ fact_order_item（订单项事实）
  │  ├─ order_item_id (BK)
  │  ├─ order_id (FK)
  │  ├─ product_id (FK → dim_product)
  │  ├─ quantity
  │  ├─ unit_price
  │  ├─ discount
  │  ├─ amount
  │  └─ (粒度：一行一个订单项)
  │
  ├─ fact_daily_sales（每日销售快照）
  │  ├─ date_id (FK → dim_date)
  │  ├─ category_id (FK → dim_product)
  │  ├─ channel_id (FK → dim_channel)
  │  ├─ total_sales
  │  ├─ order_count
  │  ├─ avg_order_value
  │  ├─ customer_count
  │  └─ (粒度：日期+类别+渠道)
  │
  └─ fact_inventory（库存事实）
     ├─ inventory_id (BK)
     ├─ product_id (FK → dim_product)
     ├─ location_id (FK → dim_location)
     ├─ date_id (FK → dim_date)
     ├─ stock_qty
     ├─ reserved_qty
     ├─ available_qty
     └─ (粒度：日期+产品+仓库)
```

### 关键决策

```
Q1: 为什么要有 fact_order 和 fact_order_item 两个事实表？
A:
  ├─ fact_order：粒度粗（一个订单），用于订单级分析
  ├─ fact_order_item：粒度细（一个订单项），用于产品级分析
  ├─ 两个表都是必要的，支持不同的分析维度

Q2: 为什么 dim_product 包含 list_price 和 cost_price？
A:
  ├─ SCD Type 2 追踪价格变化
  ├─ 支持历史成本分析
  ├─ 支持毛利率追踪

Q3: 为什么 dim_date 预生成而不是在运行时生成？
A:
  ├─ 性能：避免运行时计算
  ├─ 一致性：日期维度全局一致
  ├─ 易维护：可以预设假期等
```

---

## 案例 2：SaaS 多租户数据建模

### 挑战

```
多租户系统的建模挑战：
  ├─ 数据隔离：不同租户数据不能混淆
  ├─ 查询性能：不能为每个租户创建单独的表
  ├─ 扩展性：新租户加入不能改表结构
  ├─ 成本控制：存储和计算成本要合理
```

### 解决方案 1：在每个表中添加 tenant_id

```sql
CREATE TABLE products (
  product_id INT,
  tenant_id  INT,      -- 关键：租户隔离字段
  name       VARCHAR,
  ...
  PRIMARY KEY (tenant_id, product_id),
  INDEX idx_tenant (tenant_id)
);

优点：
  ✅ 简单
  ✅ 易于查询

缺点：
  ❌ 每个查询都要加 WHERE tenant_id = ?
  ❌ 容易漏掉 WHERE 导致数据泄露
  ❌ 索引膨胀
```

### 解决方案 2：行级安全（RLS）

```sql
-- 在数据库层实现租户隔离

CREATE POLICY products_isolation ON products
  USING (tenant_id = current_setting('app.current_tenant_id')::INT);

优点：
  ✅ 在数据库层强制隔离
  ✅ 即使应用代码忘记加 WHERE 也不会泄露数据
  ✅ 开发人员不需要每次都手动加过滤条件

缺点：
  ❌ 需要数据库支持（PostgreSQL 等）
  ❌ 配置复杂
```

### 解决方案 3：逻辑数据库（Logical Database）

```
为每个租户维护一个虚拟数据库的映射

租户 1 → database_tenant_1
  ├─ products
  ├─ orders
  └─ customers

租户 2 → database_tenant_2
  ├─ products
  ├─ orders
  └─ customers

优点：
  ✅ 完全隔离
  ✅ 性能好（可以独立扩展）
  ✅ 简单易懂

缺点：
  ❌ 跨租户分析困难
  ❌ 管理成本高（需要管理多个数据库）
  ❌ 存储成本高（数据重复）
```

### 推荐方案

```
组合使用：
  ├─ 大多数表：在字段中添加 tenant_id + 在应用层检查
  ├─ 敏感表：使用数据库 RLS
  ├─ 共享数据（产品目录）：独立的共享表，无 tenant_id
  └─ 跨租户分析：专用的数据分析库，包含脱敏后的数据
```

---

## 案例 3：时间序列数据建模

### 场景

```
监控系统：每秒记录 1000 台服务器的 CPU、内存、磁盘等指标
  ├─ 数据量：1000 台 × 1000 指标 × 86400 秒/天 = 864 亿条/天
  ├─ 存储：约 100GB/天
  ├─ 查询：需要支持秒级查询
```

### 反面教材

```
❌ 错误设计 1：单一 metrics 表

CREATE TABLE metrics (
  timestamp BIGINT,
  server_id INT,
  metric_name VARCHAR,
  metric_value FLOAT,
  ...
);

问题：
  ├─ 表的行数太多（864 亿条/天）
  ├─ 查询时需要扫描大量数据
  ├─ 索引膨胀
  └─ 性能极差

❌ 错误设计 2：为每个指标创建一个表

CREATE TABLE cpu_metrics (...)
CREATE TABLE memory_metrics (...)
CREATE TABLE disk_metrics (...)
...（1000+ 个表）

问题：
  ├─ 表数量爆炸
  ├─ 维护成本高
  └─ 查询多个指标需要 JOIN 多个表
```

### 推荐设计：按时间和服务器分区

```sql
CREATE TABLE metrics (
  timestamp BIGINT,
  server_id INT,
  cpu       FLOAT,
  memory    FLOAT,
  disk      FLOAT,
  network   FLOAT,
  ...（所有指标作为列）
  PRIMARY KEY (timestamp, server_id)
)
PARTITIONED BY RANGE (timestamp)
(
  PARTITION p_2024_06_01 VALUES LESS THAN (1717200000),
  PARTITION p_2024_06_02 VALUES LESS THAN (1717286400),
  ...
);

优点：
  ✅ 表结构简洁
  ✅ 按时间分区便于查询和清理
  ✅ 大多数查询只需要扫描一个分区
  ✅ 旧数据可以转移到冷存储

查询示例：
  SELECT * FROM metrics
  WHERE timestamp BETWEEN T1 AND T2
    AND server_id = 123
  -- 只扫描相关时间分区
```

### 使用时间序列数据库

```
对于超大规模场景，使用专用时间序列数据库：

InfluxDB：
  ├─ 设计：为时间序列优化
  ├─ 存储：列式存储，压缩率高
  ├─ 查询：InfluxQL，易于时间查询
  ├─ 保留期：支持自动删除过期数据
  └─ 成本：中等

Prometheus：
  ├─ 设计：监控专用
  ├─ 存储：本地或远程
  ├─ 查询：PromQL，强大的聚合函数
  ├─ 保留期：配置后自动删除
  └─ 成本：低（开源）

TimescaleDB（PostgreSQL 扩展）：
  ├─ 优点：SQL 兼容
  ├─ 性能：优化的时间序列查询
  ├─ 存储：自动分区
  └─ 成本：低
```

---

## 案例 4：图数据建模

### 场景

```
社交网络：用户之间有关注关系、评论关系等

传统关系模型：
  ├─ users 表：用户信息
  ├─ follows 表：user_id → follower_id
  └─ 查询"A 的朋友的朋友"需要多次 JOIN
```

### 使用图数据库

```
Neo4j 建模：

节点类型：
  ├─ User（用户）
  │  ├─ user_id
  │  ├─ name
  │  └─ created_at
  │
  ├─ Post（帖子）
  │  ├─ post_id
  │  ├─ content
  │  └─ created_at
  │
  └─ Tag（标签）
     ├─ tag_id
     └─ name

关系类型：
  ├─ FOLLOWS（A 关注 B）
  ├─ LIKES（A 赞了 B 的帖子）
  ├─ COMMENTS（A 评论了 B 的帖子）
  ├─ HAS_TAG（帖子有标签）
  └─ SIMILAR_TO（两个用户相似）

查询示例：
  // 找出 A 的朋友的朋友
  MATCH (a:User {id:'A'})-[:FOLLOWS]->(f1)-[:FOLLOWS]->(f2)
  RETURN DISTINCT f2

优点：
  ✅ 查询简洁
  ✅ 性能好（图遍历优化）
  ✅ 易于表达复杂关系
```

---

## 案例 5：非结构化数据建模

### 场景

```
日志系统、文本分析等需要存储和查询非结构化数据
```

### 使用半结构化字段

```sql
CREATE TABLE documents (
  doc_id INT,
  title VARCHAR,
  content TEXT,
  metadata JSON,      -- 半结构化字段
  created_at DATETIME,
  ...
);

-- 查询 JSON 字段
SELECT * FROM documents
WHERE metadata->>'source' = 'web'
  AND CAST(metadata->>'views' AS INT) > 1000;

优点：
  ✅ 灵活存储
  ✅ 查询相对简单

缺点：
  ❌ 查询性能一般
  ❌ 难以索引
```

### 使用搜索引擎

```
Elasticsearch 建模：

Index: documents
Type: _doc

Mapping:
{
  "properties": {
    "doc_id": {"type": "keyword"},
    "title": {"type": "text"},
    "content": {"type": "text"},
    "tags": {"type": "keyword"},
    "created_at": {"type": "date"},
    "metadata": {"type": "object"}
  }
}

优点：
  ✅ 全文搜索性能好
  ✅ 聚合分析能力强
  ✅ 分布式扩展性好

缺点：
  ❌ 相对复杂
  ❌ 需要额外的基础设施
```

---

## 最佳实践总结

```
1. 从业务需求出发
   ✅ 理解数据的使用模式
   ✅ 与业务人员充分沟通

2. 选择合适的数据模型
   ✅ OLTP：3NF，规范化
   ✅ OLAP：星形/雪花模型
   ✅ 时间序列：按时间分区
   ✅ 图：使用图数据库
   ✅ 文本：使用搜索引擎

3. 性能优化
   ✅ 适度反范式化
   ✅ 合理分区和分片
   ✅ 有效的索引策略
   ✅ 及时的统计信息更新

4. 可扩展性设计
   ✅ 提前考虑数据增长
   ✅ 设计分区和分片方案
   ✅ 避免单点故障

5. 安全性考虑
   ✅ 敏感数据加密
   ✅ 访问控制（RLS）
   ✅ 审计日志
   ✅ 合规性检查

6. 文档和维护
   ✅ 详细的数据字典
   ✅ 清晰的设计文档
   ✅ 定期的架构评估
   ✅ 团队知识转移
```

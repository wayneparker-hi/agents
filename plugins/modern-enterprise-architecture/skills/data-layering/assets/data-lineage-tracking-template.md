# 数据血缘追踪模板与工具

## 概述

数据血缘追踪（Data Lineage）是数据治理的关键环节。它记录数据从源系统到最终应用的整个生命周期，帮助我们：
- 追踪数据来源
- 分析数据影响范围
- 进行根因分析
- 保证数据合规性

---

## 数据血缘追踪的三个层次

### 层次 1：表级血缘（Table-Level Lineage）

**概念：** 记录表与表之间的依赖关系

```
源系统表 → ODS 表 → DIM/FACT 表 → AGG 表 → ADS 表 → 应用

示例：
  source_order_table
       ↓ (ETL: daily_load_order)
  ods_order_order
       ↓ (ETL: transform_order_data)
  fact_order
       ↓ (SQL: agg_order_query)
  agg_daily_sales
       ↓ (API: sales_dashboard_api)
  销售看板应用
```

**实施方式：**
```
维护一个血缘映射表：

source_table          | target_table      | etl_job_name        | frequency  | data_owner
──────────────────────┼──────────────────┼────────────────────┼────────────┼──────────
order_table (ERP)     | ods_order_order  | daily_load_order   | 每天      | 数据部
order_table (ERP)     | ods_order_order  | cdc_sync_order     | 实时      | 数据部
ods_order_order       | fact_order       | transform_fact_    | 每天      | 数据部
fact_order            | agg_daily_sales  | agg_sales_summary  | 每小时    | 数据部
```

### 层次 2：字段级血缘（Column-Level Lineage）

**概念：** 记录字段之间的依赖关系

```
关键指标：销售额
  ├─ 定义：SUM(fact_order.amount)
  ├─ 源字段：fact_order.amount
  ├─ 转换规则：
  │  ├─ 排除取消订单（status != 'CANCELLED'）
  │  ├─ 排除退款（refund_amount = 0）
  │  └─ 转换为 RMB（amount * exchange_rate）
  ├─ 源头追溯：ods_order_order.order_amount
  ├─ 业务定义：实际收入
  └─ 使用应用：销售看板、财务报表
```

**实施方式：**
```sql
-- 维护字段血缘映射表

CREATE TABLE data_lineage_column (
  lineage_id         INT PRIMARY KEY,
  source_table       VARCHAR(100),
  source_column      VARCHAR(100),
  target_table       VARCHAR(100),
  target_column      VARCHAR(100),
  transformation     TEXT,        -- 转换逻辑
  dependency_type    VARCHAR(20), -- 直接/间接
  etl_job_id         INT,
  created_date       DATE,
  updated_date       DATE
);

示例记录：
  source_table='ods_order_order'
  source_column='amount'
  target_table='fact_order'
  target_column='order_amount'
  transformation='amount * exchange_rate'
  dependency_type='直接'
```

### 层次 3：业务逻辑血缘（Business Logic Lineage）

**概念：** 记录业务指标和数据的关系

```
业务指标：日均销售额（Daily Average Sales）
  ├─ 定义：Σ(订单金额) / 有订单的天数
  ├─ 计算过程：
  │  ├─ Step 1：从 fact_order 获取所有订单金额
  │  ├─ Step 2：过滤取消和退款订单
  │  ├─ Step 3：按日期分组求和
  │  ├─ Step 4：计算平均值
  │  └─ Step 5：展示在销售看板上
  ├─ 数据质量依赖：
  │  ├─ 订单表完整性 > 99%
  │  ├─ 金额字段准确率 > 99%
  │  └─ 及时性 < 1 小时
  ├─ 使用者：销售部、财务部
  └─ 更新频率：每小时

映射：
  日均销售额 ← SUM(fact_order.amount) / COUNT(DISTINCT fact_order.order_date)
                                ↑
  fact_order ← 来自 ods_order_order
                                ↑
  ods_order_order ← 来自 ERP order_table
```

---

## 数据血缘追踪的实施方法

### 方法 1：手动维护血缘文档

**适合场景：** 数据系统规模不大，流程相对稳定

**实施步骤：**

```
Step 1：创建血缘文档模板
Step 2：在 ETL 代码中添加血缘标注
Step 3：定期更新和维护
Step 4：在代码审查时验证血缘
```

**血缘文档模板：**

```markdown
# 订单数据血缘文档

## 表级血缘

### ODS 层：ods_order_order

来源系统：ERP（订单管理系统）
来源表：order_table
加载方式：每天 02:00 全量加载 + 增量更新
ETL 任务：daily_load_order, cdc_sync_order
数据量：100M 行
主要列：
  - order_id（订单号）← source.order_id
  - customer_id（客户号）← source.customer_id
  - amount（订单金额）← source.amount
  - status（订单状态）← source.status

### DW 层：fact_order

来源表：ods_order_order
转换规则：
  - 排除 status='CANCELLED' 的订单
  - amount 转换为 RMB（* exchange_rate）
  - 按订单粒度聚合
维度：customer_id, product_id, date_id
度量值：quantity, amount, discount
加载频率：每天 03:00
ETL 任务：transform_fact_order

### AGG 层：agg_daily_sales

来源表：fact_order
聚合维度：date, customer_id
聚合指标：total_sales=SUM(amount), order_count=COUNT(*)
加载频率：每小时
目标应用：销售看板

## 字段级血缘

### 关键指标：日总销售额

定义：SUM(fact_order.amount) WHERE date=TODAY
源字段链路：
  amount (agg_daily_sales)
    ↓ (SUM聚合)
  order_amount (fact_order)
    ↓ (转换：* exchange_rate)
  amount (ods_order_order)
    ↓ (抽取)
  amount (ERP order_table)

质量保证：
  - ODS 完整性 > 99%
  - 与 ERP 对账一致
  - 及时性 < 1 小时

## 业务逻辑血缘

### 销售目标达成率

定义：日总销售额 / 日销售目标
使用应用：销售看板、KPI 报表
使用部门：销售部、财务部、高管
影响范围：10+ 个报表，5+ 个系统
```

**优点：**
- 简单，无需额外工具
- 易于理解和沟通
- 便于版本控制（与代码一起）

**缺点：**
- 容易过期和不同步
- 需要手动维护
- 难以进行自动化分析

### 方法 2：通过 ETL 代码自动生成血缘

**适合场景：** 有完善的 ETL 框架，能够自动追踪血缘

**实施步骤：**

```
Step 1：在 ETL 工具中配置血缘追踪
Step 2：在每个 ETL 任务中记录输入/输出表
Step 3：自动生成和更新血缘关系
Step 4：构建血缘可视化
```

**示例：使用 Apache NiFi 的血缘追踪**

```
在 NiFi 中，每个 Processor 都会自动记录：
  - 输入数据流（input）
  - 处理逻辑（transformation）
  - 输出数据流（output）
  - 处理时间
  - 处理的数据量

NiFi 会自动生成血缘图，显示数据从源到目的地的完整流向。
```

**示例：使用 Talend 的血缘追踪**

```
在 Talend 中：
  - 每个 Job 自动追踪输入/输出表
  - 自动生成血缘文档
  - 提供血缘查询接口
  - 支持影响分析（某表变化影响哪些应用）
```

### 方法 3：使用专门的血缘追踪工具

**工具选项：**

```
1. Apache Atlas（开源）
   ├─ 功能：表级和字段级血缘
   ├─ 集成：Hadoop/Hive/Spark
   ├─ 成本：免费，需要部署和维护
   └─ 使用场景：大规模数据中台

2. DataHub（LinkedIn 开源）
   ├─ 功能：完整的数据治理平台
   ├─ 血缘追踪：表级和字段级
   ├─ 成本：免费
   └─ 使用场景：综合数据治理

3. Collibra（商业工具）
   ├─ 功能：企业级数据治理
   ├─ 血缘追踪：完整支持
   ├─ 成本：昂贵
   └─ 使用场景：大型企业

4. Alation（商业工具）
   ├─ 功能：数据目录 + 血缘追踪
   ├─ 成本：昂贵
   └─ 使用场景：大型企业数据治理

5. 国内工具
   ├─ 阿里数据中台产品
   ├─ 腾讯数据大脑
   ├─ 美团大数据平台
   └─ 字节火山引擎
```

---

## 数据血缘追踪的常见应用

### 应用场景 1：根因分析

**问题：** 销售看板的销售额数据突然下降 30%

**血缘追踪流程：**

```
1. 从销售看板查询出问题的数据：agg_daily_sales[2024-06-15]
   ├─ 预期值：1000 万
   ├─ 实际值：700 万
   └─ 差异：300 万（-30%）

2. 追踪数据来源：agg_daily_sales ← fact_order
   ├─ fact_order[2024-06-15] 的数据是否正常？
   ├─ 检查：应该有 5000 订单，实际 3500 订单
   └─ 初步判断：fact_order 数据异常

3. 继续追踪：fact_order ← ods_order_order
   ├─ ods_order_order[2024-06-15] 的数据是否完整？
   ├─ 检查：应该有 5000 订单，实际 5000 订单
   ├─ ods_order_order 数据正常，问题在转换逻辑
   └─ 发现：有 1500 订单被过滤掉了（status='CANCELLED'）

4. 进一步分析：
   ├─ 查询 ERP 系统，订单状态是否更新？
   ├─ 结果：发现 ERP 一个批量更新脚本错误
   ├─ 批量将订单状态改为 CANCELLED
   └─ 原因找到

5. 解决方案：
   ├─ 在 ERP 中修复错误数据
   ├─ 重新加载 ODS 数据
   ├─ 重新计算 fact_order 和 AGG 表
   ├─ 验证销售看板数据恢复正常
```

### 应用场景 2：影响分析

**问题：** 计划修改 product 维度表，添加新的属性 "profit_margin"

**血缘追踪流程：**

```
1. 查询 dim_product 被谁使用：
   ├─ fact_order 使用 product_id
   ├─ agg_product_sales 使用 product_id
   ├─ agg_product_category_sales 使用 product_id
   └─ ... (共 15 个 fact/agg 表使用)

2. 进一步查询这些表被哪些应用使用：
   ├─ sales_dashboard（销售看板）
   ├─ finance_report（财务报表）
   ├─ product_analysis（产品分析）
   ├─ ...

3. 评估影响范围：
   ├─ 涉及应用数：15 个
   ├─ 涉及用户数：200+ 人
   ├─ 修改难度：中等（需要更新部分聚合逻辑）
   └─ 建议：发起变更管理流程

4. 变更计划：
   ├─ 与相关方沟通
   ├─ 制定测试计划
   ├─ 安排灰度发布
   └─ 监控变更风险
```

### 应用场景 3：数据合规性审计

**问题：** 验证客户数据是否只被授权应用使用

**血缘追踪流程：**

```
1. 找出所有使用客户数据的表：
   ├─ ods_customer_customer（ODS）
   ├─ dim_customer（维度）
   ├─ fact_order（事实）
   ├─ agg_customer_behavior（聚合）
   └─ ads_customer_360（ADS）

2. 进一步找出使用这些表的应用：
   ├─ sales_crm（销售 CRM）
   ├─ marketing_platform（营销平台）
   ├─ financial_system（财务系统）
   ├─ 第三方数据分析 ← ⚠️ 未授权！

3. 发现问题：
   ├─ 第三方数据分析平台可以访问客户隐私数据
   ├─ 需要立即采取行动

4. 修复措施：
   ├─ 移除第三方对 dim_customer 的访问
   ├─ 创建脱敏的客户维度（仅含非敏感字段）
   ├─ 更新第三方应用使用脱敏维度
   ├─ 加强访问控制
   └─ 定期审计
```

---

## 血缘追踪的最佳实践

```
1. 早期规划
   ✅ 在数据分层设计初期就考虑血缘追踪
   ✅ 选择合适的工具/方法
   ✅ 制定血缘管理规范

2. 自动化优先
   ✅ 优先使用工具自动生成血缘
   ✅ 减少手动维护的工作量
   ✅ 保证血缘的及时性和准确性

3. 多层级记录
   ✅ 同时记录表级、字段级、业务逻辑级血缘
   ✅ 支持不同层级的查询和分析

4. 可视化展示
   ✅ 提供清晰的血缘可视化
   ✅ 便于理解和沟通
   ✅ 支持交互式探索

5. 定期验证
   ✅ 定期审计血缘的准确性
   ✅ 及时更新变化的血缘
   ✅ 建立血缘质量规范

6. 与其他治理结合
   ✅ 结合数据质量管理
   ✅ 结合访问控制
   ✅ 结合数据分类
   └─ 构建完整的数据治理体系
```

---

## 血缘追踪表格模板

### 表级血缘追踪表

```
| 源表 | 源系统 | 目标表 | ETL任务 | 数据频率 | 是否测试 | 负责人 | 备注 |
|------|--------|--------|---------|---------|---------|--------|------|
| order_table | ERP | ods_order_order | daily_load | 每天 02:00 | ✅ | 张三 | |
| ods_order_order | ODS | fact_order | transform_fact | 每天 03:00 | ✅ | 李四 | |
| fact_order | DW | agg_daily_sales | agg_daily | 每小时 | ✅ | 王五 | |
```

### 字段级血缘追踪表

```
| 业务指标 | 源字段 | 目标字段 | 转换逻辑 | 质量规则 | 使用应用 |
|----------|--------|---------|---------|---------|---------|
| 日总销售额 | ods_order_order.amount | fact_order.amount | amount * exchange_rate | 完整性>99% | 销售看板 |
```

---

## 血缘追踪的常见问题

### Q1：如何处理复杂的 ETL 逻辑中的血缘？

**A：**
- 在 ETL 代码中添加清晰的注释
- 使用血缘追踪工具的高级功能
- 必要时补充额外的文档说明

### Q2：如何保证血缘追踪的及时性？

**A：**
- 使用自动化工具追踪
- 在代码审查时验证血缘
- 定期检查血缘的准确性
- 建立血缘变更通知机制

### Q3：血缘追踪会增加维护成本吗？

**A：**
短期增加成本，但长期收益：
- 故障诊断更快（降低 MTTR）
- 变更风险评估更准确
- 数据治理更完善
- ROI 通常为 1:3 或更高
```

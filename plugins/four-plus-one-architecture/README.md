# 4+1架构视图设计Plugin

基于Philippe Kruchten的4+1架构视图方法论的完整Claude Code plugin，提供系统化的架构设计指导、PlantUML绘图规范、实战工作流和完整示例。

## 🎯 核心价值

- **五大视图体系**：场景、逻辑、开发、进程、物理，完整的架构描述方法
- **方法论完整**：从概念到实践，包含详细指南和检查清单
- **规范化指导**：基于PlantUML规范指南的标准化绘图规范
- **实战导向**：所有方法都包含模板、示例和最佳实践
- **智能协作**：6个专业架构师agents的协作支持

## 🚀 快速开始

### 最快速的方式（10分钟）

```
/41view-master-architect
我需要为一个电商平台设计完整的4+1架构视图
```

这会自动帮您：
1. 分析系统需求和约束
2. 推荐设计方法和工具
3. 引导进行完整架构设计

### 按步骤的方式（8-12周）

**步骤 1：需求分析与架构约束（1-2周）**
```
/41view-master-architect
帮我分析系统需求和确定架构约束
```

**步骤 2：场景视图设计（1周）**
```
/scenario-view-architect
帮我识别系统的核心用例和关键场景
```

**步骤 3：逻辑视图设计（2周）**
```
/logical-view-architect
帮我设计系统的类和对象模型
```

**步骤 4：开发视图设计（2周）**
```
/development-view-architect
帮我进行模块分解和分层架构设计
```

**步骤 5：进程视图设计（1-2周）**
```
/process-view-architect
帮我分析系统并发性和设计性能优化
```

**步骤 6：物理视图设计（1-2周）**
```
/physical-view-architect
帮我规划系统部署和基础设施
```

**步骤 7：视图整合与评审（1-2周）**
```
/41view-master-architect
帮我整合所有视图并进行架构评审
```

## 📚 核心功能

### Agents（6个专业架构师）

#### 视图特定Agents
- **scenario-view-architect** - 场景视图架构师，负责用例识别和场景分析
- **logical-view-architect** - 逻辑视图架构师，负责类和对象设计
- **development-view-architect** - 开发视图架构师，负责模块和分层设计
- **process-view-architect** - 进程视图架构师，负责并发和性能设计
- **physical-view-architect** - 物理视图架构师，负责部署和基础设施设计

#### 协调Agent
- **41view-master-architect** - 4+1总协调师，负责视图整合和架构评审

### Skills（8个核心知识模块）

#### 五大视图Skills
1. **scenario-view-design** - 用例图和场景设计
2. **logical-view-design** - 类图和对象图设计
3. **development-view-design** - 组件图和包图设计
4. **process-view-design** - 序列图和活动图设计
5. **physical-view-design** - 部署图和网络拓扑设计

#### 支撑Skills
6. **plantuml-best-practices** - PlantUML绘图规范和最佳实践
7. **41view-integration** - 视图整合方法和一致性检查
8. **architecture-documentation** - 架构文档编写规范

### Commands（4个工作流）

- **/41view-complete-workflow** - 完整4+1架构设计工作流（8-12周）
- **/logical-development-workflow** - 逻辑+开发视图快速工作流（3-4周）
- **/process-physical-workflow** - 进程+物理视图工作流（2-3周）
- **/architecture-review** - 架构评审工作流（1周）

## 🎓 使用场景

### 场景 1：新项目启动
**目标**：为新项目设计完整的系统架构

```
/41view-complete-workflow

输入：
- 项目名称：电商平台 2.0
- 项目范围：在线商城 + 支付系统 + 库存管理
- 用户规模：日活 50 万+
- 交付周期：6 个月
- 质量要求：99.99% 可用性
```

**输出**：
- 5个完整的架构视图（PlantUML图表）
- 完整的架构文档
- ADR（架构决策记录）
- 实施路线图

### 场景 2：快速架构梳理
**目标**：对现有系统进行快速架构分析

```
/logical-development-workflow

输入：
- 现有代码结构
- 技术栈信息
- 性能要求
```

**输出**：
- 逻辑视图和开发视图
- 模块分析报告
- 改进建议

### 场景 3：性能优化设计
**目标**：通过架构优化来解决性能问题

```
/process-physical-workflow

输入：
- 现有性能瓶颈
- 扩展性需求
- 基础设施约束
```

**输出**：
- 进程视图和物理视图
- 并发设计方案
- 部署优化建议

### 场景 4：架构评审
**目标**：对现有或新建架构进行专业评审

```
/architecture-review

输入：
- 架构文档或图表
- 系统约束和需求
- 遇到的问题
```

**输出**：
- 评审报告
- 问题清单
- 改进建议
- 优先级排序

## 🔧 实战示例

### 示例 1：电商平台架构

完整的电商系统4+1架构设计示例，包含：
- **场景视图** - 用户购物、支付、售后等用例
- **逻辑视图** - 用户、订单、商品、支付等聚合
- **开发视图** - API网关、微服务分解、分层架构
- **进程视图** - 订单流程、支付流程、库存扣减
- **物理视图** - Kubernetes部署、数据库集群、缓存方案

位置: `assets/examples/ecommerce-system/`

### 示例 2：在线教育平台

在线教育系统的4+1架构设计，包含：
- **场景视图** - 学生上课、老师授课、作业批改等用例
- **逻辑视图** - 课程、学生、作业、成绩等领域模型
- **开发视图** - 直播服务、课程服务、评分服务等微服务
- **进程视图** - 直播连接、文件上传、成绩计算流程
- **物理视图** - CDN分发、实时通信、数据存储方案

位置: `assets/examples/online-education/`

### 示例 3：微服务平台

微服务架构的标准4+1设计，包含：
- **场景视图** - 跨域操作的用例
- **逻辑视图** - 微服务间的数据模型
- **开发视图** - 微服务拆分和组织
- **进程视图** - 服务间通信和异步处理
- **物理视图** - 容器编排和服务网格

位置: `assets/examples/microservices-platform/`

## 📖 详细文档导航

### 快速查找

| 我想要... | 使用... | 预计时间 |
|---------|--------|--------|
| 快速了解4+1视图 | `/41view-master-architect` | 10分钟 |
| 学习场景视图设计 | `/scenario-view-architect` + `skills/scenario-view-design/` | 1小时 |
| 学习逻辑视图设计 | `/logical-view-architect` + `skills/logical-view-design/` | 2小时 |
| 学习PlantUML规范 | `skills/plantuml-best-practices/` | 1.5小时 |
| 完整架构设计 | `/41view-complete-workflow` | 8-12周 |
| 架构评审 | `/architecture-review` | 1周 |
| 查看实战示例 | `assets/examples/` | 2小时 |

## 💡 4+1架构视图简介

### 五大视图概览

```
┌─────────────────────────────────────┐
│         场景视图 (Scenarios)        │
│           +1 视图                   │
│    用例驱动，整合其他4个视图        │
└─────────────────────────────────────┘
         ↓        ↓        ↓        ↓
    ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐
    │逻辑视图│ │开发视图│ │进程视图│ │物理视图│
    │Logical │ │Develop │ │Process │ │Physical│
    └────────┘ └────────┘ └────────┘ └────────┘
```

### 1️⃣ 逻辑视图 (Logical View)
**关注点**：系统的功能和对象
**目标人群**：最终用户、分析师、测试人员
**输出物**：类图、对象图、状态图、序列图

### 2️⃣ 开发视图 (Development View)
**关注点**：软件模块的组织结构
**目标人群**：程序员、软件管理员
**输出物**：组件图、包图、分层架构图

### 3️⃣ 进程视图 (Process View)
**关注点**：系统运行时的并发性和性能
**目标人群**：系统集成商、性能工程师
**输出物**：序列图、活动图、流程图

### 4️⃣ 物理视图 (Physical View)
**关注点**：系统部署和网络拓扑
**目标人群**：系统工程师、运维人员
**输出物**：部署图、网络拓扑图

### 5️⃣ 场景视图 (Scenarios / Use Case View)
**关注点**：驱动架构设计的关键场景
**目标人群**：所有干系人
**输出物**：用例图、场景描述

## 🎯 最佳实践

### 使用Agents的建议

1. **逐步设计**
   - 先从场景视图开始（理解需求）
   - 再设计逻辑视图（定义模型）
   - 然后开发视图（规划结构）
   - 接着进程视图（优化性能）
   - 最后物理视图（规划部署）

2. **充分沟通**
   - 每个视图都要与团队讨论
   - 确保理解一致
   - 记录关键决策

3. **文档并发**
   - 一边设计一边记录ADR
   - 形成完整架构文档
   - 便于知识传承

4. **定期评审**
   - 完成每个视图后进行评审
   - 使用41view-master-architect进行整体评审
   - 确保各视图一致性

### 使用Skills的建议

1. **由浅入深**
   - 先读SKILL.md了解概念
   - 再看assets中的模板
   - 最后查看references深度学习

2. **立即实践**
   - 使用提供的PlantUML模板
   - 跟随检查清单完成设计
   - 用实际项目验证学到的方法

3. **避免常见陷阱**
   - 参考常见错误对照表
   - 学习最佳实践
   - 查看实战示例

## 🔗 与其他框架的关系

### 与MEAF的关系
- 4+1架构视图可作为MEAF中**应用架构层**的具体实现方法
- 逻辑视图对应MEAF的**领域模型**
- 开发视图对应MEAF的**应用边界设计**
- 进程视图对应MEAF的**服务编排**
- 物理视图对应MEAF的**部署架构**

### 与DDD的关系
- 逻辑视图可使用DDD的**聚合、实体、值对象**概念
- 支持**领域驱动设计**的具体实现
- 便于识别**限界上下文**

### 与C4模型的关系
- 逻辑视图 ↔ C4 Context + Container
- 开发视图 ↔ C4 Container + Component
- 进程视图 ↔ C4 Dynamic Diagrams
- 物理视图 ↔ C4 Deployment Diagrams

## 📦 Plugin内容清单

### 已实现
- ✅ 6个Agents（完整实现）
- ✅ 8个Skills（包含模板和参考）
- ✅ 4个Commands工作流
- ✅ 3个完整实战示例
- ✅ PlantUML规范指南
- ✅ 架构文档模板

### 计划实现
- 🔄 AI驱动的架构图生成
- 🔄 架构文档自动化
- 🔄 更多实战案例库
- 🔄 视频教程和工作坊

## 🚦 快速导航

| 使用场景 | 推荐入口 | 完成时间 |
|---------|--------|--------|
| 了解4+1方法 | `/41view-master-architect` | 10分钟 |
| 学习用例设计 | `/scenario-view-architect` | 1小时 |
| 学习类设计 | `/logical-view-architect` | 2小时 |
| 学习模块划分 | `/development-view-architect` | 2小时 |
| 学习性能优化 | `/process-view-architect` | 1.5小时 |
| 学习部署设计 | `/physical-view-architect` | 1.5小时 |
| 完整设计工作流 | `/41view-complete-workflow` | 8-12周 |
| 快速设计 | `/logical-development-workflow` | 3-4周 |
| 性能优化 | `/process-physical-workflow` | 2-3周 |
| 架构评审 | `/architecture-review` | 1周 |
| 查看示例 | `assets/examples/` | 2小时 |
| 学习PlantUML | `skills/plantuml-best-practices/` | 1.5小时 |

## 💬 使用示例

### 示例 1：快速了解4+1架构

```
User: 什么是4+1架构视图？我应该如何应用它？

Agent (41view-master-architect):
4+1架构视图是由Philippe Kruchten在1995年提出的软件架构设计方法。
它通过5个并发的视角来描述系统架构：

1️⃣ 场景视图（+1）- 用例驱动，整合其他4个视图
2️⃣ 逻辑视图 - 系统的功能和对象
3️⃣ 开发视图 - 软件的模块组织
4️⃣ 进程视图 - 系统运行时的并发性
5️⃣ 物理视图 - 系统的部署和拓扑

每个视图都针对不同的干系人，解决不同的关注点...
```

### 示例 2：完整架构设计

```
User: 我需要为我们的电商平台设计完整的架构

Agent (41view-master-architect):
很好！我来帮您进行完整的4+1架构设计。

首先，让我了解几个关键信息：

1. **业务规模**：日活用户、订单量、并发量？
2. **技术约束**：现有技术栈、团队规模？
3. **质量需求**：可用性、性能、安全性要求？
4. **时间计划**：设计周期、上线时间？

基于这些信息，我会帮您：
✓ 分析系统需求和约束
✓ 设计5个完整视图
✓ 生成PlantUML架构图
✓ 编写完整架构文档
✓ 提供实施建议
```

## 📞 反馈与支持

如在使用过程中：
- 发现任何问题，请提交Issue
- 有改进建议，欢迎讨论
- 想分享成功案例，我们很乐意听

## 📄 参考资源

### 官方文献
- Kruchten, P. (1995). "The 4+1 View Model of Software Architecture"
- Kruchten, P. (2004). "The Rational Unified Process: An Introduction"

### 扩展学习
- C4 Model (https://c4model.com/)
- Domain-Driven Design (Eric Evans)
- Software Architecture Pattern (Mark Richards)

### 相关Plugin
- **domain-driven-design** - DDD战略和战术设计
- **modern-enterprise-architecture** - MEAF企业架构框架

## 📄 许可证

本Plugin基于4+1架构视图的公开知识和最佳实践，遵循MIT许可证。

---

**让我们用科学的方法设计更好的软件架构！** 🎯

**Plugin版本**: 1.0.0
**最后更新**: 2025年11月11日
**维护者**: 架构设计社区

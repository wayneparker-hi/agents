# Domain-Driven Design Plugin

完整的领域驱动设计（DDD）插件，包含战略设计、战术设计、建模方法和实践指导。

## 组件概览

### Agents（4 个）
专业化的 DDD 顾问，提供各个方面的指导：

1. **strategic-designer** - 战略设计专家
   - 限界上下文识别与设计
   - 上下文映射关系
   - 统一语言建立
   - 团队组织设计

2. **domain-modeler** - 领域建模专家
   - 聚合设计原则
   - 实体和值对象模式
   - 领域行为建模
   - 聚合间通信

3. **architecture-advisor** - 架构顾问
   - 分层架构设计
   - CQRS 和事件溯源
   - 六边形架构
   - 微服务边界设计

4. **ubiquitous-language-facilitator** - 统一语言促进者
   - 术语表建立和管理
   - 跨团队术语同步
   - 技术-业务语言桥接
   - 语言验证和治理

### Skills（8 个）
结构化的知识模块，提供详细的模式、最佳实践和检查清单：

1. **bounded-context-design** - 限界上下文设计
2. **aggregate-design-principles** - 聚合设计原则
3. **event-storming** - 事件风暴方法论
4. **entity-value-object-patterns** - 实体与值对象模式
5. **context-mapping-patterns** - 上下文映射模式
6. **domain-events** - 领域事件设计
7. **ubiquitous-language** - 统一语言
8. **layered-architecture** - 分层架构

### Commands（3 个）
完整的工作流，指导实践应用：

1. **bounded-context-mapping** - 限界上下文识别与映射工作流
   - 8 个阶段的完整工作流
   - 包括业务分析、团队组织、架构决策
   - 输出：上下文地图、术语表、架构决策记录

2. **domain-modeling-workflow** - 领域建模完整工作流
   - 11 个阶段，从事件风暴到代码框架
   - 聚合设计、实体建模、测试设计
   - 输出：聚合设计、代码框架、测试计划

3. **ddd-refactoring** - 遗留系统 DDD 重构工作流
   - 8 个阶段的渐进式重构
   - 防腐层设计、灰度发布
   - 输出：重构路线图、迁移计划、经验总结

## 使用指南

### 场景 1：新项目的 DDD 设计

```bash
# 1. 启动战略设计
# 激活：strategic-designer agent
"我要为电商系统设计限界上下文"

# 2. 运行完整工作流
/bounded-context-mapping --domain=电商 --complexity=复杂

# 3. 进行领域建模
/domain-modeling-workflow --subdomain=订单管理 --modeling-method=event-storming
```

### 场景 2：遗留系统重构

```bash
# 分析现有系统并规划重构
/ddd-refactoring --legacy-type=monolith --migration-strategy=strangler
```

### 场景 3：建立统一语言

```bash
# 激活：ubiquitous-language-facilitator
"团队对业务术语理解不一致，怎么统一？"

# 自动加载 ubiquitous-language skill
```

## 核心概念

### 战略设计层面
- **限界上下文**：清晰的业务边界，独立自治的系统单元
- **上下文映射**：定义上下文间的协作关系
- **统一语言**：技术和业务人员的共同理解

### 战术设计层面
- **聚合**：一致性边界，保护业务不变量
- **实体和值对象**：领域对象的两种基本类型
- **领域服务**：跨聚合的领域逻辑
- **领域事件**：异步通信，支持最终一致性

### 架构模式
- **分层架构**：UI、应用、领域、基础设施四层
- **六边形架构**：端口和适配器模式
- **CQRS**：命令查询职责分离
- **事件溯源**：事件作为唯一的真实来源

## 文件结构

```
domain-driven-design/
├── agents/                          # 4 个专业化 agents
│   ├── strategic-designer.md
│   ├── domain-modeler.md
│   ├── architecture-advisor.md
│   └── ubiquitous-language-facilitator.md
├── skills/                          # 8 个知识模块
│   ├── bounded-context-design/
│   │   ├── SKILL.md
│   │   ├── references/              # 详细参考
│   │   └── assets/                  # 模板和清单
│   ├── aggregate-design-principles/
│   ├── event-storming/
│   ├── entity-value-object-patterns/
│   ├── context-mapping-patterns/
│   ├── domain-events/
│   ├── ubiquitous-language/
│   └── layered-architecture/
├── commands/                        # 3 个工作流
│   ├── bounded-context-mapping.md
│   ├── domain-modeling-workflow.md
│   └── ddd-refactoring.md
└── README.md                        # 本文件
```

## 快速开始

### 步骤 1：理解基础概念
- 阅读 `docs/plugins/domain-driven-design.md` 了解概览
- 浏览 skills 中的 Core Concepts 部分

### 步骤 2：选择应用场景
- 新系统设计 → 使用 bounded-context-mapping 和 domain-modeling-workflow
- 遗留系统改造 → 使用 ddd-refactoring
- 团队沟通 → 使用 ubiquitous-language 和 ubiquitous-language-facilitator

### 步骤 3：运用实践
- 使用工作流指导逐步实施
- 激活相关 skills 深入学习
- 使用 agents 获取专业指导

## 核心价值

✓ **完整的知识体系**：从战略到战术，从理论到实践
✓ **可执行的工作流**：不仅是概念，而是可操作的步骤
✓ **专业化指导**：4 个专家 agent，不同角度的深度指导
✓ **实用工具**：检查清单、模板、示例代码
✓ **团队协作**：支持多角色、跨职能的协作

## 参考资源

- **docs/plugins/domain-driven-design.md** - 完整的计划文档
- **每个 skill 的 references/** - 详细的参考文档
- **每个 skill 的 assets/** - 可用的模板和清单

## 贡献和反馈

这个插件基于 DDD 的核心原理，欢迎反馈和改进建议。

---

**创建时间**：2024-11-10
**版本**：1.0
**覆盖**：DDD 战略设计、战术设计、建模方法、架构模式

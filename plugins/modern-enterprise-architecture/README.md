# MEAF: Modern Enterprise Architecture Framework Plugin

基于 ThoughtWorks 现代企业架构框架（MEAF）的完整 Claude Code plugin，提供从战略规划到技术实现的企业架构设计全流程指导。

## 🎯 核心价值

- **四层架构体系**：业务 → 应用 → 数据 → 技术，完整的架构设计流程
- **方法论完整**：Event Storming、DDD、质量属性分析、数据分层等核心方法
- **实战导向**：所有方法都包含检查清单、模板和真实案例
- **敏捷轻量**：持续改进而非一次做对，快速迭代的架构设计方法
- **智能指导**：10+ 企业架构师 agents 的专业建议和协作

## 🚀 快速开始

### 最快速的方式（5 分钟）

```
/meaf-master-architect
我需要为一个电商平台设计完整的企业架构
```

这会自动帮您：
1. 分析业务需求和系统边界
2. 推荐适用的架构设计方法
3. 引导逐步完成架构设计

### 按步骤的方式（1-2 小时）

**步骤 1：业务架构设计**
```
/business-architect
帮我识别电商平台的核心业务能力和领域
```

**步骤 2：应用架构设计**
```
/application-architect
根据业务能力，帮我设计应用系统的边界和分解
```

**步骤 3：数据架构设计**
```
/data-architect
为我设计数据分层和一致性策略
```

**步骤 4：技术架构设计**
```
/technology-architect
帮我选择技术栈和部署架构
```

## 📚 核心功能

### Agents（10+ 企业架构师）

#### 业务架构 Agents
- **business-architect** - 业务架构师，统筹业务架构设计全流程
- **domain-expert** - 领域建模专家，深度指导 DDD 战术设计
- **capability-designer** - 能力设计师，指导三层能力结构设计

#### 应用架构 Agents
- **application-architect** - 应用架构师，指导应用分解和边界划分
- **quality-attribute-analyzer** - 质量属性分析师，识别和解决架构冲突
- **integration-designer** - 集成设计师，指导应用间集成设计

#### 数据架构 Agents
- **data-architect** - 数据架构师，指导数据分层和架构设计
- **data-modeler** - 数据建模师，指导逻辑和物理数据模型设计

#### 技术架构 Agents
- **technology-architect** - 技术架构师，指导技术选型和部署设计
- **adr-facilitator** - ADR 促进者，指导架构决策记录

#### 编排 Agent
- **meaf-master-architect** - MEAF 总架构师，智能路由和协调四层架构设计

### Skills（15+ 核心知识模块）

#### 业务架构 Skills
1. **event-storming** - 快速识别领域事件和聚合的方法论
2. **domain-modeling** - DDD 战术设计（聚合、实体、值对象）
3. **process-modeling** - 流程建模和可变性分析
4. **capability-modeling** - 三层能力结构设计（L1 域/L2 能力/L3 组件）
5. **subdomain-classification** - 核心域、支撑域、通用域识别

#### 应用架构 Skills
6. **application-boundary-design** - 应用边界划分和四种应用类型
7. **quality-attributes-conflicts** - 七类质量属性冲突识别与解决
8. **application-grouping** - 基于子域和能力的应用分组
9. **integration-patterns** - API、消息、事件集成模式

#### 数据架构 Skills
10. **data-layering** - 数据分层设计（四层架构：ODS/DWD/DWS/ADS）
11. **data-modeling** - 逻辑和物理数据模型设计
12. **data-consistency** - 强一致性 vs 最终一致性的选择

#### 技术架构 Skills
13. **technology-selection** - 四维度技术选型评估框架
14. **deployment-architecture** - 部署架构和 CI/CD 设计
15. **architecture-decision-records** - ADR 编写和管理

### Commands（7+ 工作流）

#### 业务架构工作流
- **/business-arch-workflow** - 完整业务架构设计工作流（Event Storming → Domain Modeling → Process Modeling → Capability Modeling）
- **/event-storming-workshop** - Event Storming 工作坊引导（3 小时标准议程）

#### 应用架构工作流
- **/application-arch-workflow** - 完整应用架构设计工作流

#### 数据架构工作流
- **/data-arch-workflow** - 完整数据架构设计工作流

#### 技术架构工作流
- **/tech-arch-workflow** - 完整技术架构设计工作流

#### 编排工作流
- **/meaf-complete-workflow** - 四层企业架构完整工作流（从业务到技术）
- **/meaf-architecture-review** - 架构评审和质量检查

## 🎓 使用场景

### 场景 1：新项目启动
**目标**：为新项目设计完整的企业架构

```
/meaf-complete-workflow

输入：
- 项目名称：电商平台 1.0
- 项目范围：在线商城 + 支付系统
- 用户规模：日活 10 万+
- 交付周期：6 个月
```

### 场景 2：仅需业务架构
**目标**：快速识别业务能力和主要流程

```
/business-arch-workflow

输入：
- 业务场景描述
- 关键角色和流程
- 性能要求
```

**输出**：
- 事件风暴分析结果
- 领域模型
- 流程模型
- 能力地图

### 场景 3：架构评审
**目标**：评审已有架构的合理性和改进建议

```
/meaf-architecture-review

输入：
- 现有架构文档
- 系统边界
- 遇到的问题
```

**输出**：
- 评审报告
- 改进建议
- 优先级排序

### 场景 4：处理架构冲突
**目标**：识别和解决质量属性冲突

```
/quality-attribute-analyzer

输入：
- 需要优化的质量属性（性能、安全、可用性等）
- 当前的约束和限制
- 业务需求优先级
```

**输出**：
- 冲突分析
- 解决策略
- 架构决策

## 🔧 使用示例

### 示例 1：完整的业务架构设计

```
User: 我们要设计一个互联网金融平台，包括账户管理、资产管理、交易、风控等核心能力。帮我进行 Event Storming

Agent (business-architect):
我会指导您通过 Event Storming 来快速识别平台的关键领域事件和聚合。

首先，让我们确定几个关键要素：

1. 参与者：谁应该参与这个工作坊？
   - 产品经理、技术架构师、业务分析师、开发团队代表...

2. 时间：建议 3-4 小时的工作坊

3. 关键流程（初步推断）：
   - 用户注册 → 身份验证 → 账户开设
   - 资产充值 → 购买 → 支付结算
   - 风险监测 → 风险决策 → 风险规避

现在让我们开始详细的 Event Storming...
```

### 示例 2：应用架构设计

```
User: 根据业务架构，我需要设计应用系统的分解。有 3 个核心领域：账户、交易、风控

Agent (application-architect):
基于您的业务能力，我建议以下应用分解：

┌─────────────────────────────────────────────────┐
│           用户界面应用（Portal App）             │
└────────────┬────────────────────────────────────┘
             │
    ┌────────┼────────┬──────────┐
    │        │        │          │
┌───▼──┐ ┌──▼──┐ ┌──▼────┐ ┌──▼──┐
│账户  │ │交易  │ │风控   │ │报表  │
│应用  │ │应用  │ │应用   │ │应用  │
└───┬──┘ └──┬──┘ └──┬────┘ └──┬──┘
    │      │       │        │
    └──────┼───────┼────────┘
           │
       ┌──▼──────────┐
       │  基础服务   │
       │  (Auth/Msg) │
       └─────────────┘

接下来，我们需要分析质量属性的冲突...
```

## 📖 详细文档

### 每个 Skill 的结构

每个 skill 都包含完整的学习和实践内容：

```
{skill-name}/
├── SKILL.md                    # 核心知识（5-10 分钟阅读）
│   ├── 核心概念
│   ├── 分步指南
│   ├── 最佳实践
│   ├── 常见陷阱
│   └── 检查清单
│
├── references/                 # 深度参考（15-30 分钟阅读）
│   ├── 详细的概念讲解
│   ├── 高级模式
│   └── 实战经验
│
└── assets/                     # 实践工具（直接可用）
    ├── 模板
    ├── 检查清单
    ├── 决策矩阵
    └── 实际案例
```

### 快速查找

**我想学习 Event Storming**
- `skills/event-storming/SKILL.md` - 快速了解
- `skills/event-storming/references/` - 深度学习
- `skills/event-storming/assets/event-storming-template.md` - 使用模板

**我需要设计应用架构**
- `/application-architect` - 咨询架构师
- `skills/application-boundary-design/SKILL.md` - 学习方法
- `/quality-attribute-analyzer` - 分析冲突

**我需要设计数据架构**
- `/data-architect` - 咨询数据架构师
- `skills/data-layering/SKILL.md` - 学习分层设计
- `skills/data-modeling/SKILL.md` - 学习建模方法

## 🎯 MEAF 的核心理念

### 三大设计原则

1. **业务驱动** - 战略与业务价值驱动，而非技术驱动
2. **敏捷轻量** - 持续改进而非一次做对，快速迭代
3. **可落地** - 从实践出发而非从理论推导，立即可用

### 四层架构体系

```
业务架构
  ↓ 支撑
应用架构
  ↓ 依赖
数据架构
  ↓ 运行于
技术架构
```

每一层都有明确的:
- **输入**：上一层的设计决策
- **方法论**：核心的设计方法（2-4 个）
- **输出**：下一层的输入和约束

## 💡 最佳实践

### 使用 Agents 的建议

1. **逐层设计**：先业务 → 应用 → 数据 → 技术，避免跳跃
2. **充分沟通**：每一层都要与相关 agents 深入讨论，确保理解一致
3. **文档并发**：一边设计一边记录决策，形成 Architecture Decision Record (ADR)
4. **评审检验**：完成每个阶段后进行评审，验证合理性

### 使用 Skills 的建议

1. **由浅入深**：先读 SKILL.md 了解概念，再看 references 深度学习
2. **立即实践**：使用 assets 中的模板进行实际设计，边做边学
3. **收集问题**：记录在应用 skill 时遇到的问题，为后续改进提供反馈

### 使用 Commands 的建议

1. **完整工作流优先**：使用 `/meaf-complete-workflow` 了解整个流程
2. **按需选择**：针对特定层次，使用对应的 workflow commands
3. **循环迭代**：企业架构不是一次性的，使用 `/meaf-architecture-review` 定期评审和优化

## 🔗 与现有 MEAF 命令的关系

本 plugin 与现有的 `/meaf-*` 命令体系充分集成：

- **Plugin 的 Agents** → 提供对话式的深度指导
- **Plugin 的 Skills** → 结构化的知识体系和实践指南
- **Plugin 的 Commands** → 调用现有的 `/meaf-*` 命令，进行智能编排

因此，您可以：
- 在 Claude Code 中无缝切换 plugin agents 和 `/meaf-*` 命令
- 使用 plugin 作为学习和指导工具
- 使用 `/meaf-*` 命令作为具体的执行工具

## 📦 Plugin 内容清单

### 已实现（Phase 1-2）
- ✅ 基础结构和文档
- ✅ 3 个业务架构 Agents
- ✅ 5 个业务架构 Skills（完整版）
- ✅ 2 个业务架构 Workflow Commands

### 计划实现（Phase 3-6）
- 🔄 3 个应用架构 Agents
- 🔄 4 个应用架构 Skills
- 🔄 2 个数据架构 Agents
- 🔄 3 个数据架构 Skills
- 🔄 2 个技术架构 Agents
- 🔄 3 个技术架构 Skills
- 🔄 1 个 Master Orchestration Agent
- 🔄 实战案例和详细示例

## 🚦 快速导航

| 我想要... | 使用... | 预计时间 |
|---------|--------|--------|
| 快速了解 MEAF | `/meaf-master-architect` | 5 分钟 |
| 学习 Event Storming | `/domain-expert` + `skills/event-storming/` | 30 分钟 |
| 完整的业务架构设计 | `/business-arch-workflow` | 2-4 小时 |
| 完整的企业架构设计 | `/meaf-complete-workflow` | 2-3 天 |
| 解决架构冲突 | `/quality-attribute-analyzer` | 1-2 小时 |
| 架构评审 | `/meaf-architecture-review` | 1-2 小时 |
| 寻找模板和清单 | `skills/{skill-name}/assets/` | 5-10 分钟 |

## 📞 反馈与改进

如在使用过程中发现任何问题、有改进建议或想分享成功案例，欢迎：
- 提交 Issue
- 分享您的架构设计案例
- 建议新增的 skills 或改进方向

## 📄 许可证

本 Plugin 基于 MEAF 框架的公开知识，遵循相应的开源许可证。

---

**让我们用科学的方法设计更好的企业架构！** 🎯

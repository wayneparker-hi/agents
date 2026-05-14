# Architecture & Design Principles

此市场遵循行业最佳实践，注重粒度、可组合性和最小化 token 使用。

## 核心理念

### 单一职责原则

- 每个插件**做好一件事**（Unix 哲学）
- 明确、专注的目的（可用 5-10 个词描述）
- 平均插件大小：**5.5 个组件**（遵循 Anthropic 的 2-8 模式）
- **无臃肿插件** - 所有插件都专注且有目的性

### 可组合性优于捆绑

- 根据需要混合和匹配插件
- 工作流编排器组合专注插件
- 无强制功能捆绑
- 插件间边界清晰

### 上下文效率

- 更小的工具 = 更快的处理速度
- 更好地适应 LLM 上下文窗口
- 更准确、专注的响应
- 仅安装所需内容

### 可维护性

- 单一目的 = 更易更新
- 边界清晰 = 隔离变更
- 减少重复 = 更简单的维护
- 依赖隔离

## 粒度插件架构

### 插件分布

- **81 个专注插件**（80 个本地 + 1 个通过 git-subdir 的外部插件），针对特定用例优化
- **25 个清晰类别**，每个类别 1-10 个插件，便于发现
- 按领域组织：
  - **开发**：4 个插件（调试、后端、前端、多平台）
  - **安全**：4 个插件（扫描、合规、后端 API、前端移动）
  - **运维**：4 个插件（事件、诊断、分布式、可观测性）
  - **语言**：7 个插件（Python、JS/TS、系统、JVM、脚本、函数式、嵌入式）
  - **基础设施**：5 个插件（部署、验证、K8s、云、CI/CD）
  - 以及 18 个更多专业类别

### 组件分解

**185 个专业 Agents**

- 领域专家，具有深度知识
- 跨架构、语言、基础设施、质量、数据/AI、文档、业务和 SEO 组织
- 模型优化，采用三层策略（Opus、Sonnet、Haiku），实现性能和成本优化

**15 个工作流编排器**

- 多代理协调系统
- 复杂操作，如全栈开发、安全加固、ML 管道、事件响应
- 预配置的代理工作流

**71 个开发工具**

- 优化的实用程序，包括：
  - 项目脚手架（Python、TypeScript、Rust）
  - 安全扫描（SAST、依赖审计、XSS）
  - 测试生成（pytest、Jest）
  - 组件脚手架（React、React Native）
  - 基础设施设置（Terraform、Kubernetes）

**107 个 Agent Skills**

- 模块化知识包
- 渐进式披露架构
- 跨 18 个插件的领域特定专业知识
- 规范兼容（Anthropic Agent Skills 规范）

## 仓库结构

```
claude-agents/
├── .claude-plugin/
│   └── marketplace.json          # 市场目录（77 个插件）
├── plugins/                       # 隔离的插件目录
│   ├── python-development/
│   │   ├── agents/               # Python 语言 agents
│   │   │   ├── python-pro.md
│   │   │   ├── django-pro.md
│   │   │   └── fastapi-pro.md
│   │   ├── commands/             # Python 工具
│   │   │   └── python-scaffold.md
│   │   └── skills/               # Python skills（共 5 个）
│   │       ├── async-python-patterns/
│   │       ├── python-testing-patterns/
│   │       ├── python-packaging/
│   │       ├── python-performance-optimization/
│   │       └── uv-package-manager/
│   ├── backend-development/
│   │   ├── agents/
│   │   │   ├── backend-architect.md
│   │   │   ├── graphql-architect.md
│   │   │   └── tdd-orchestrator.md
│   │   ├── commands/
│   │   │   └── feature-development.md
│   │   └── skills/               # 后端 skills（共 3 个）
│   │       ├── api-design-principles/
│   │       ├── architecture-patterns/
│   │       └── microservices-patterns/
│   ├── security-scanning/
│   │   ├── agents/
│   │   │   └── security-auditor.md
│   │   ├── commands/
│   │   │   ├── security-hardening.md
│   │   │   ├── security-sast.md
│   │   │   └── security-dependencies.md
│   │   └── skills/               # 安全 skills（共 1 个）
│   │       └── sast-configuration/
│   ├── c4-architecture/
│   │   ├── agents/               # C4 架构 agents
│   │   │   ├── c4-code.md
│   │   │   ├── c4-component.md
│   │   │   ├── c4-container.md
│   │   │   └── c4-context.md
│   │   └── commands/
│   │       └── c4-architecture.md
│   └── ... (62 个更多隔离插件)
├── docs/                          # 文档
│   ├── agent-skills.md           # Agent Skills 指南
│   ├── agents.md                 # Agent 参考
│   ├── plugins.md                # 插件目录
│   ├── usage.md                  # 使用指南
│   └── architecture.md           # 此文件
└── README.md                      # 快速开始
```

## 插件结构

每个插件包含：

- **agents/** - 该领域的专业 agents（可选）
- **commands/** - 该插件特定的工具和工作流（可选）
- **skills/** - 渐进式披露的模块化知识包（可选）

### 最低要求

- 至少一个 agent 或一个 command
- 明确、专注的目的
- 所有文件中的适当 frontmatter
- marketplace.json 中的条目

### 示例插件

```
plugins/kubernetes-operations/
├── agents/
│   └── kubernetes-architect.md   # K8s 架构和设计
├── commands/
│   └── k8s-deploy.md            # 部署自动化
└── skills/
    ├── k8s-manifest-generator/   # 清单创建 skill
    ├── helm-chart-scaffolding/   # Helm 图表 skill
    ├── gitops-workflow/          # GitOps 自动化 skill
    └── k8s-security-policies/    # 安全策略 skill
```

## Agent Skills 架构

### 渐进式披露

Skills 使用三层架构实现 token 效率：

1. **元数据** (Frontmatter)：名称和激活条件（始终加载）
2. **指令**：核心指导和模式（激活时加载）
3. **资源**：示例和模板（按需加载）

### 规范合规性

所有 skills 都遵循 [Agent Skills 规范](https://github.com/anthropics/skills/blob/main/agent_skills_spec.md)：

```yaml
---
name: skill-name                  # 必需：连字符命名
description: skill 的作用。使用时机 [触发器]。 # 必需：< 1024 字符
---

# 带有渐进式披露的 skill 内容
```

### 优势

- **Token 效率**：仅在需要时加载相关知识
- **专业化的专业知识**：深度领域知识而无冗余
- **清晰激活**：明确的触发器防止不必要的调用
- **可组合性**：跨工作流混合和匹配 skills
- **可维护性**：隔离更新不影响其他 skills

详情请参见 [Agent Skills](./agent-skills.md) 了解 153 个 skills 的完整详情。

## 模型配置策略

### 四层架构

系统战略性地使用 Claude Opus、Sonnet、Haiku 和 Inherit 分配：

| 模型 | 数量 | 使用场景 |
|-------|-------|----------|
| Opus | 54 个 agents | 关键架构、安全、代码审查 |
| Sonnet | 62 个 agents | 复杂任务、智能支持 |
| Haiku | 20 个 agents | 快速运维任务 |
| Inherit | 49 个 agents | 将模型选择推迟到用户在运行时决定 |

### 选择标准

**Haiku - 快速执行与确定性任务**

- 根据明确定义的规范生成代码
- 遵循既定模式创建测试
- 使用清晰模板编写文档
- 执行基础设施操作
- 执行数据库查询优化
- 处理客户支持响应
- 处理 SEO 优化任务
- 管理部署管道

**Sonnet - 复杂推理与架构**

- 设计系统架构
- 做出技术选择决策
- 执行安全审计
- 审查代码的架构模式
- 创建复杂的 AI/ML 管道
- 提供语言特定的专业知识
- 编排多代理工作流
- 处理关键业务的法律/人力资源事务

### 混合编排

结合模型以实现最佳性能和成本：

```
规划阶段 (Sonnet) → 执行阶段 (Haiku) → 审查阶段 (Sonnet)

示例：
backend-architect (Sonnet) 设计 API
  ↓
生成端点 (Haiku) 实现规范
  ↓
test-automator (Haiku) 创建测试
  ↓
code-reviewer (Sonnet) 验证架构
```

## 性能与质量

### 优化的 Token 使用

- **隔离插件** 仅加载所需内容
- **粒度架构** 减少不必要的上下文
- **渐进式披露** (skills) 按需加载知识
- **清晰边界** 防止上下文污染

### 组件覆盖

- **100% agent 覆盖** - 所有插件至少包含一个 agent
- **100% 组件可用性** - 所有 185 个 agents 在插件中可访问
- **高效分布** - 平均每个插件 5.5 个组件

### 可发现性

- **清晰的插件名称** 立即传达目的
- **逻辑分类** 23 个明确定义的类别
- **可搜索的文档** 带有交叉引用
- **易于找到** 适合工作的正确工具

## 设计模式

### 模式 1：单一目的插件

每个插件专注于一个领域：

```
python-development/
├── agents/           # Python 语言专家
├── commands/         # Python 项目脚手架
└── skills/           # Python 特定知识
```

**优势：**

- 责任明确
- 易于维护
- 最小化 token 使用
- 与其他插件可组合

### 模式 2：工作流编排

编排器插件协调多个 agents：

```
full-stack-orchestration/
└── commands/
    └── full-stack-feature.md    # 协调 7+ 个 agents
```

**编排：**

1. backend-architect（设计 API）
2. database-architect（设计模式）
3. frontend-developer（构建 UI）
4. test-automator（创建测试）
5. security-auditor（安全审查）
6. deployment-engineer（CI/CD）
7. observability-engineer（监控）

### 模式 3：Agent + Skill 集成

Agents 提供推理，skills 提供知识：

```
用户："使用异步模式构建 FastAPI 项目"
  ↓
fastapi-pro agent（编排）
  ↓
fastapi-templates skill（提供模式）
  ↓
python-scaffold command（生成项目）
```

### 模式 4：多插件组合

复杂工作流使用多个插件：

```
功能开发工作流：
1. backend-development:feature-development
2. security-scanning:security-hardening
3. unit-testing:test-generate
4. comprehensive-review:full-review
5. cicd-automation:workflow-automate
6. observability-monitoring:monitor-setup
```

## 版本控制与更新

### 市场更新

- `.claude-plugin/marketplace.json` 中的市场目录
- 插件的语义版本控制
- 维护向后兼容性
- 为重大变更提供清晰的迁移指南

### 插件更新

- 单个插件更新不影响其他插件
- Skills 可以独立更新
- Agents 可以添加/删除而不破坏工作流
- Commands 保持稳定的接口

## 贡献指南

### 添加插件

1. 创建插件目录：`plugins/{plugin-name}/`
2. 添加 agents 和/或 commands
3. 可选添加 skills
4. 更新 marketplace.json
5. 在适当类别中记录文档

### 添加 Agent

1. 创建 `plugins/{plugin-name}/agents/{agent-name}.md`
2. 添加 frontmatter（名称、描述、模型）
3. 编写全面的系统提示
4. 更新插件定义

### 添加 Skill

1. 创建 `plugins/{plugin-name}/skills/{skill-name}/SKILL.md`
2. 添加 YAML frontmatter（名称、带"Use when"的描述）
3. 使用渐进式披露编写 skill 内容
4. 将其添加到 marketplace.json 中的插件 skills 数组

### 质量标准

- **清晰命名** - 连字符命名，描述性
- **专注范围** - 单一职责
- **完整文档** - 什么、何时、如何
- **测试功能** - 提交前验证
- **规范合规** - 遵循 Anthropic 指南

## 参见

- [Agent Skills](./agent-skills.md) - 模块化知识包
- [Agent Reference](./agents.md) - 完整 agent 目录
- [Plugin Reference](./plugins.md) - 所有 77 个插件
- [Usage Guide](./usage.md) - 命令和工作流

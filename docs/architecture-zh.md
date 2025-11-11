# Architecture & Design Principles

此市场遵循行业最佳实践，注重粒度、可组合性和最小化token使用。

## 核心理念

### 单一职责原则

- 每个插件**做好一件事**（Unix哲学）
- 明确、专注的目的（可用5-10个词描述）
- 平均插件大小：**3.4个组件**（遵循Anthropic的2-8模式）
- **无臃肿插件** - 所有插件都专注且有目的性

### 可组合性优于捆绑

- 根据需要混合和匹配插件
- 工作流编排器组合专注插件
- 无强制功能捆绑
- 插件间边界清晰

### 上下文效率

- 更小的工具 = 更快的处理速度
- 更好地适应LLM上下文窗口
- 更准确、专注的响应
- 仅安装所需内容

### 可维护性

- 单一目的 = 更易更新
- 边界清晰 = 隔离变更
- 减少重复 = 更简单的维护
- 依赖隔离

## 粒度插件架构

### 插件分布

- **63个专注插件**，针对特定用例优化
- **23个清晰类别**，每个类别1-6个插件，便于发现
- 按领域组织：
  - **开发**：4个插件（调试、后端、前端、多平台）
  - **安全**：4个插件（扫描、合规、后端API、前端移动）
  - **运维**：4个插件（事件、诊断、分布式、可观测性）
  - **语言**：7个插件（Python、JS/TS、系统、JVM、脚本、函数式、嵌入式）
  - **基础设施**：5个插件（部署、验证、K8s、云、CI/CD）
  - 以及18个更多专业类别

### 组件分解

**85个专业Agents**
- 领域专家，具有深度知识
- 跨架构、语言、基础设施、质量、数据/AI、文档、业务和SEO组织
- 模型优化（47个Haiku，97个Sonnet），实现性能和成本优化

**15个工作流编排器**
- 多代理协调系统
- 复杂操作，如全栈开发、安全加固、ML管道、事件响应
- 预配置的代理工作流

**44个开发工具**
- 优化的实用程序，包括：
  - 项目脚手架（Python、TypeScript、Rust）
  - 安全扫描（SAST、依赖审计、XSS）
  - 测试生成（pytest、Jest）
  - 组件脚手架（React、React Native）
  - 基础设施设置（Terraform、Kubernetes）

**47个Agent Skills**
- 模块化知识包
- 渐进式披露架构
- 跨14个插件的领域特定专业知识
- 规范兼容（Anthropic Agent Skills规范）

## 仓库结构

```
claude-agents/
├── .claude-plugin/
│   └── marketplace.json          # 市场目录（63个插件）
├── plugins/                       # 隔离的插件目录
│   ├── python-development/
│   │   ├── agents/               # Python语言agents
│   │   │   ├── python-pro.md
│   │   │   ├── django-pro.md
│   │   │   └── fastapi-pro.md
│   │   ├── commands/             # Python工具
│   │   │   └── python-scaffold.md
│   │   └── skills/               # Python skills（共5个）
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
│   │   └── skills/               # 后端skills（共3个）
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
│   │   └── skills/               # 安全skills（共1个）
│   │       └── sast-configuration/
│   └── ... (60个更多隔离插件)
├── docs/                          # 文档
│   ├── agent-skills.md           # Agent Skills指南
│   ├── agents.md                 # Agent参考
│   ├── plugins.md                # 插件目录
│   ├── usage.md                  # 使用指南
│   └── architecture.md           # 此文件
└── README.md                      # 快速开始
```

## 插件结构

每个插件包含：

- **agents/** - 该领域的专业agents（可选）
- **commands/** - 该插件特定的工具和工作流（可选）
- **skills/** - 渐进式披露的模块化知识包（可选）

### 最低要求

- 至少一个agent或一个command
- 明确、专注的目的
- 所有文件中的适当frontmatter
- marketplace.json中的条目

### 示例插件

```
plugins/kubernetes-operations/
├── agents/
│   └── kubernetes-architect.md   # K8s架构和设计
├── commands/
│   └── k8s-deploy.md            # 部署自动化
└── skills/
    ├── k8s-manifest-generator/   # 清单创建skill
    ├── helm-chart-scaffolding/   # Helm图表skill
    ├── gitops-workflow/          # GitOps自动化skill
    └── k8s-security-policies/    # 安全策略skill
```

## Agent Skills架构

### 渐进式披露

Skills使用三层架构实现token效率：

1. **元数据** (Frontmatter)：名称和激活条件（始终加载）
2. **指令**：核心指导和模式（激活时加载）
3. **资源**：示例和模板（按需加载）

### 规范合规性

所有skills都遵循[Agent Skills规范](https://github.com/anthropics/skills/blob/main/agent_skills_spec.md)：

```yaml
---
name: skill-name                  # 必需：连字符命名
description: skill的作用。使用时机[触发器]。 # 必需：< 1024字符
---

# 带有渐进式披露的skill内容
```

### 优势

- **Token效率**：仅在需要时加载相关知识
- **专业化的专业知识**：深度领域知识而无冗余
- **清晰激活**：明确的触发器防止不必要的调用
- **可组合性**：跨工作流混合和匹配skills
- **可维护性**：隔离更新不影响其他skills

详情请参见[Agent Skills](./agent-skills.md)了解47个skills的完整详情。

## 模型配置策略

### 双层架构

系统战略性地使用Claude Opus和Sonnet模型：

| 模型 | 数量 | 使用场景 |
|-------|-------|----------|
| Haiku | 47个agents | 快速执行、确定性任务 |
| Sonnet | 97个agents | 复杂推理、架构决策 |

### 选择标准

**Haiku - 快速执行与确定性任务**
- 根据明确定义的规范生成代码
- 遵循既定模式创建测试
- 使用清晰模板编写文档
- 执行基础设施操作
- 执行数据库查询优化
- 处理客户支持响应
- 处理SEO优化任务
- 管理部署管道

**Sonnet - 复杂推理与架构**
- 设计系统架构
- 做出技术选择决策
- 执行安全审计
- 审查代码的架构模式
- 创建复杂的AI/ML管道
- 提供语言特定的专业知识
- 编排多代理工作流
- 处理关键业务的法律/人力资源事务

### 混合编排

结合模型以实现最佳性能和成本：

```
规划阶段 (Sonnet) → 执行阶段 (Haiku) → 审查阶段 (Sonnet)

示例：
backend-architect (Sonnet) 设计API
  ↓
生成端点 (Haiku) 实现规范
  ↓
test-automator (Haiku) 创建测试
  ↓
code-reviewer (Sonnet) 验证架构
```

## 性能与质量

### 优化的Token使用

- **隔离插件** 仅加载所需内容
- **粒度架构** 减少不必要的上下文
- **渐进式披露** (skills) 按需加载知识
- **清晰边界** 防止上下文污染

### 组件覆盖

- **100% agent覆盖** - 所有插件至少包含一个agent
- **100% 组件可用性** - 所有85个agents在插件中可访问
- **高效分布** - 平均每个插件3.4个组件

### 可发现性

- **清晰的插件名称** 立即传达目的
- **逻辑分类** 23个明确定义的类别
- **可搜索的文档** 带有交叉引用
- **易于找到** 适合工作的正确工具

## 设计模式

### 模式1：单一目的插件

每个插件专注于一个领域：

```
python-development/
├── agents/           # Python语言专家
├── commands/         # Python项目脚手架
└── skills/           # Python特定知识
```

**优势：**
- 责任明确
- 易于维护
- 最小化token使用
- 与其他插件可组合

### 模式2：工作流编排

编排器插件协调多个agents：

```
full-stack-orchestration/
└── commands/
    └── full-stack-feature.md    # 协调7+个agents
```

**编排：**
1. backend-architect (设计API)
2. database-architect (设计模式)
3. frontend-developer (构建UI)
4. test-automator (创建测试)
5. security-auditor (安全审查)
6. deployment-engineer (CI/CD)
7. observability-engineer (监控)

### 模式3：Agent + Skill集成

Agents提供推理，skills提供知识：

```
用户："使用异步模式构建FastAPI项目"
  ↓
fastapi-pro agent (编排)
  ↓
fastapi-templates skill (提供模式)
  ↓
python-scaffold command (生成项目)
```

### 模式4：多插件组合

复杂工作流使用多个插件：

```
功能开发工作流：
1. backend-development:feature-development
2. security-scanning:security-hardening
3. unit-testing:test-generate
4. code-review-ai:ai-review
5. cicd-automation:workflow-automate
6. observability-monitoring:monitor-setup
```

## 版本控制与更新

### 市场更新

- `.claude-plugin/marketplace.json`中的市场目录
- 插件的语义版本控制
- 维护向后兼容性
- 为重大变更提供清晰的迁移指南

### 插件更新

- 单个插件更新不影响其他插件
- Skills可以独立更新
- Agents可以添加/删除而不破坏工作流
- Commands保持稳定的接口

## 贡献指南

### 添加插件

1. 创建插件目录：`plugins/{plugin-name}/`
2. 添加agents和/或commands
3. 可选添加skills
4. 更新marketplace.json
5. 在适当类别中记录文档

### 添加Agent

1. 创建`plugins/{plugin-name}/agents/{agent-name}.md`
2. 添加frontmatter（名称、描述、模型）
3. 编写全面的系统提示
4. 更新插件定义

### 添加Skill

1. 创建`plugins/{plugin-name}/skills/{skill-name}/SKILL.md`
2. 添加YAML frontmatter（名称、带"Use when"的描述）
3. 使用渐进式披露编写skill内容
4. 将其添加到marketplace.json中的插件skills数组

### 质量标准

- **清晰命名** - 连字符命名，描述性
- **专注范围** - 单一职责
- **完整文档** - 什么、何时、如何
- **测试功能** - 提交前验证
- **规范合规** - 遵循Anthropic指南

## 参见

- [Agent Skills](./agent-skills.md) - 模块化知识包
- [Agent Reference](./agents.md) - 完整agent目录
- [Plugin Reference](./plugins.md) - 所有63个插件
- [Usage Guide](./usage.md) - 命令和工作流
# Complete Plugin Reference

浏览所有 **80 个专注、单一用途的插件**，按类别组织，另外还有 1 个通过 `git-subdir` 市场条目分发的外部托管插件（`qa-orchestra`）— 共 81 个插件。

> 💡 **另外推荐：** [Pensyve](https://github.com/major7apps/pensyve) — Claude Code 的通用记忆运行时。从其自己的市场（`major7apps/pensyve`）分发，更新直接来自源头。安装方式：`/plugin marketplace add major7apps/pensyve`，然后 `/plugin install pensyve@major7apps-pensyve`。

## 快速开始 - 必备插件

> 💡 **刚开始？** 安装这些热门插件以立即获得生产力提升。

### 开发必备

**code-documentation** - 文档和技术写作

```bash
/plugin install code-documentation
```

自动化文档生成、代码解释和教程创建，用于全面的技术文档。

**debugging-toolkit** - 智能调试和开发者体验

```bash
/plugin install debugging-toolkit
```

交互式调试、错误分析和 DX 优化，用于更快的问题解决。

**git-pr-workflows** - Git 自动化和 PR 增强

```bash
/plugin install git-pr-workflows
```

Git 工作流自动化、拉取请求增强和团队入职流程。

### 全栈开发

**backend-development** - 后端 API 设计和架构

```bash
/plugin install backend-development
```

RESTful 和 GraphQL API 设计，采用测试驱动开发和现代后端架构模式。

**frontend-mobile-development** - UI 和移动开发

```bash
/plugin install frontend-mobile-development
```

React/React Native 组件开发，具有自动化脚手架和跨平台实现。

**full-stack-orchestration** - 端到端功能开发

```bash
/plugin install full-stack-orchestration
```

多代理协调从后端→前端→测试→安全→部署。

### 测试与质量

**unit-testing** - 自动化测试生成

```bash
/plugin install unit-testing
```

自动生成 pytest（Python）和 Jest（JavaScript）单元测试，具有全面的边缘情况覆盖。

### 基础设施与运维

**cloud-infrastructure** - 云架构设计

```bash
/plugin install cloud-infrastructure
```

AWS/Azure/GCP 架构、Kubernetes 设置、Terraform IaC 和多云成本优化。

**incident-response** - 生产事件管理

```bash
/plugin install incident-response
```

快速事件分类、根本原因分析和自动化解决工作流，用于生产系统。

### 语言支持

**python-development** - Python 项目脚手架

```bash
/plugin install python-development
```

FastAPI/Django 项目初始化，采用现代工具（uv、ruff）和生产就绪架构。

**javascript-typescript** - JavaScript/TypeScript 脚手架

```bash
/plugin install javascript-typescript
```

Next.js、React + Vite 和 Node.js 项目设置，采用 pnpm 和 TypeScript 最佳实践。

---

## 完整插件目录

### 🎨 开发（6 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **debugging-toolkit** | 交互式调试和 DX 优化 | `/plugin install debugging-toolkit` |
| **backend-development** | 后端 API 设计，包括 GraphQL 和 TDD | `/plugin install backend-development` |
| **frontend-mobile-development** | 前端 UI 和移动开发 | `/plugin install frontend-mobile-development` |
| **ui-design** | 移动（iOS、Android、React Native）和 Web 的 UI/UX 设计 | `/plugin install ui-design` |
| **multi-platform-apps** | 跨平台应用协调（web/iOS/Android） | `/plugin install multi-platform-apps` |

### 📚 文档（4 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **code-documentation** | 文档生成和代码解释 | `/plugin install code-documentation` |
| **documentation-generation** | OpenAPI 规范、Mermaid 图表、教程 | `/plugin install documentation-generation` |
| **c4-architecture** | 综合 C4 架构文档工作流，包括自底向上代码分析、组件综合、容器映射和上下文图 | `/plugin install c4-architecture` |

### 🔄 工作流（5 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **conductor** | 上下文驱动开发，包含轨道、规范和分阶段实现计划 | `/plugin install conductor` |
| **git-pr-workflows** | Git 自动化和 PR 增强 | `/plugin install git-pr-workflows` |
| **full-stack-orchestration** | 端到端功能编排 | `/plugin install full-stack-orchestration` |
| **tdd-workflows** | 测试驱动开发方法论 | `/plugin install tdd-workflows` |

### ✅ 测试（2 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **unit-testing** | 自动化单元测试生成（Python/JavaScript） | `/plugin install unit-testing` |
| **qa-orchestra** | 多代理 QA 工具包（10 个 agents、Chrome MCP 实时验证、技术栈无关）— 外部插件 | `/plugin install qa-orchestra` |

### 🔍 质量（3 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **comprehensive-review** | 多视角代码分析 | `/plugin install comprehensive-review` |
| **performance-testing-review** | 性能分析和测试覆盖率审查 | `/plugin install performance-testing-review` |

### 🛠️ 实用工具（4 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **code-refactoring** | 代码清理和技术债务管理 | `/plugin install code-refactoring` |
| **dependency-management** | 依赖审计和版本管理 | `/plugin install dependency-management` |
| **error-debugging** | 错误分析和跟踪调试 | `/plugin install error-debugging` |
| **team-collaboration** | 团队工作流和站会自动化 | `/plugin install team-collaboration` |

### 🤖 AI & ML（4 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **llm-application-dev** | LLM 应用和提示工程 | `/plugin install llm-application-dev` |
| **agent-orchestration** | 多代理系统优化 | `/plugin install agent-orchestration` |
| **context-management** | 上下文持久化和恢复 | `/plugin install context-management` |
| **machine-learning-ops** | ML 训练管道和 MLOps | `/plugin install machine-learning-ops` |

### 📊 数据（2 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **data-engineering** | ETL 管道和数据仓库 | `/plugin install data-engineering` |
| **data-validation-suite** | 模式验证和数据质量 | `/plugin install data-validation-suite` |

### 🗄️ 数据库（2 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **database-design** | 数据库架构和模式设计 | `/plugin install database-design` |
| **database-migrations** | 数据库迁移自动化 | `/plugin install database-migrations` |

### 🚨 运维（4 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **incident-response** | 生产事件管理 | `/plugin install incident-response` |
| **error-diagnostics** | 错误跟踪和根本原因分析 | `/plugin install error-diagnostics` |
| **distributed-debugging** | 分布式系统跟踪 | `/plugin install distributed-debugging` |
| **observability-monitoring** | 指标、日志、跟踪和 SLO | `/plugin install observability-monitoring` |

### ⚡ 性能（2 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **application-performance** | 应用程序分析和优化 | `/plugin install application-performance` |
| **database-cloud-optimization** | 数据库查询和云成本优化 | `/plugin install database-cloud-optimization` |

### ☁️ 基础设施（5 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **deployment-strategies** | 部署模式和回滚自动化 | `/plugin install deployment-strategies` |
| **deployment-validation** | 部署前检查和验证 | `/plugin install deployment-validation` |
| **kubernetes-operations** | K8s 清单和 GitOps 工作流 | `/plugin install kubernetes-operations` |
| **cloud-infrastructure** | AWS/Azure/GCP 云架构 | `/plugin install cloud-infrastructure` |
| **cicd-automation** | CI/CD 管道配置 | `/plugin install cicd-automation` |

### 🔒 安全（6 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **security-scanning** | SAST 分析和漏洞扫描 | `/plugin install security-scanning` |
| **security-compliance** | SOC2/HIPAA/GDPR 合规 | `/plugin install security-compliance` |
| **backend-api-security** | API 安全和认证 | `/plugin install backend-api-security` |
| **frontend-mobile-security** | XSS/CSRF 预防和移动安全 | `/plugin install frontend-mobile-security` |
| **reverse-engineering** | 二进制分析、恶意软件分类、固件安全（需授权） | `/plugin install reverse-engineering` |
| **block-no-verify** | 阻止 `--no-verify` 和 hook 绕过标志的 PreToolUse hook | `/plugin install block-no-verify` |

### 🛡️ 治理（1 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **protect-mcp** | 为每次工具调用实施 Cedar 策略执行 + Ed25519 签名收据；通过哈希链接实现离线可验证的审计跟踪 | `/plugin install protect-mcp` |

### 🔄 现代化（2 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **framework-migration** | 框架升级和迁移规划 | `/plugin install framework-migration` |
| **codebase-cleanup** | 技术债务减少和清理 | `/plugin install codebase-cleanup` |

### 🌐 API（2 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **api-scaffolding** | REST/GraphQL API 生成 | `/plugin install api-scaffolding` |
| **api-testing-observability** | API 测试和监控 | `/plugin install api-testing-observability` |

### 📢 营销（4 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **seo-content-creation** | SEO 内容写作和规划 | `/plugin install seo-content-creation` |
| **seo-technical-optimization** | 元标签、关键词和模式标记 | `/plugin install seo-technical-optimization` |
| **seo-analysis-monitoring** | 内容分析和权威性建设 | `/plugin install seo-analysis-monitoring` |
| **content-marketing** | 内容策略和网络研究 | `/plugin install content-marketing` |

### 💼 业务（4 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **business-analytics** | KPI 跟踪和财务报告 | `/plugin install business-analytics` |
| **hr-legal-compliance** | HR 政策和法律模板 | `/plugin install hr-legal-compliance` |
| **customer-sales-automation** | 支持和销售自动化 | `/plugin install customer-sales-automation` |

### 💻 语言（10 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **python-development** | Python 3.12+，包括 Django/FastAPI | `/plugin install python-development` |
| **javascript-typescript** | JavaScript/TypeScript，包括 Node.js | `/plugin install javascript-typescript` |
| **systems-programming** | Rust、Go、C、C++ 用于系统开发 | `/plugin install systems-programming` |
| **jvm-languages** | Java、Scala、C# 与企业模式 | `/plugin install jvm-languages` |
| **web-scripting** | PHP 和 Ruby 用于 Web 应用 | `/plugin install web-scripting` |
| **functional-programming** | Elixir，包括 OTP 和 Phoenix | `/plugin install functional-programming` |
| **arm-cortex-microcontrollers** | ARM Cortex-M 固件和驱动 | `/plugin install arm-cortex-microcontrollers` |

### 🔗 区块链（1 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **blockchain-web3** | 智能合约和 DeFi 协议 | `/plugin install blockchain-web3` |

### 💰 金融（1 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **quantitative-trading** | 算法交易和风险管理 | `/plugin install quantitative-trading` |

### 💳 支付（1 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **payment-processing** | Stripe/PayPal 集成和计费 | `/plugin install payment-processing` |

### 🎮 游戏（1 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **game-development** | Unity 和 Minecraft 插件开发 | `/plugin install game-development` |

### ♿ 无障碍（1 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **accessibility-compliance** | WCAG 审计和包容性设计 | `/plugin install accessibility-compliance` |

### 🎨 创意（1 个插件）

| 插件 | 描述 | 安装 |
|--------|-------------|---------|
| **meigen-ai-design** | AI 图像生成，包含创意工作流编排和提示 MCP | `/plugin install meigen-ai-design` |

## 插件结构

每个插件包含：

- **agents/** - 该领域的专业 agents
- **commands/** - 该插件特定的工具和工作流
- **skills/** - 可选的模块化知识包（渐进式披露）

示例：

```
plugins/python-development/
├── agents/
│   ├── python-pro.md
│   ├── django-pro.md
│   └── fastapi-pro.md
├── commands/
│   └── python-scaffold.md
└── skills/
    ├── async-python-patterns/
    ├── python-testing-patterns/
    ├── python-packaging/
    ├── python-performance-optimization/
    └── uv-package-manager/
```

## 安装

### 步骤 1：添加市场

```bash
/plugin marketplace add wshobson/agents
```

这使得所有 77 个插件都可用于安装，但**不会将任何 agents 或工具加载到您的上下文中**。

### 步骤 2：安装特定插件

浏览可用插件：

```bash
/plugin
```

仅安装您需要的插件：

```bash
/plugin install python-development
/plugin install backend-development
```

每个已安装的插件仅将其特定的 agents 和 commands 加载到 Claude 的上下文中。

## 插件设计原则

### 单一职责

- 每个插件**做好一件事**（Unix 哲学）
- 明确、专注的目的（可用 5-10 个词描述）
- 平均插件大小：**3.6 个组件**（遵循 Anthropic 的 2-8 模式）

### 最小化 Token 使用

- 仅安装所需内容
- 每个插件仅加载其特定的 agents 和工具
- 无不必要的资源加载到上下文中
- 通过粒度插件实现更好的上下文效率

### 可组合性

- 混合和匹配插件以实现复杂工作流
- 工作流编排器组合专注插件
- 插件间边界清晰
- 无强制功能捆绑

## 参见

- [Agent Skills](./agent-skills.md) - 跨插件的 153 个专业 skills
- [Agent Reference](./agents.md) - 完整 agent 目录
- [Usage Guide](./usage.md) - 命令和工作流
- [Architecture](./architecture.md) - 设计原则

**中文** | [English](README.md)

# Claude Code 插件：编排与自动化

> **⚡ 为 Opus 4.7、Sonnet 4.6 和 Haiku 4.5 更新** — 三层模型策略，实现最优性能

[![Run in Smithery](https://smithery.ai/badge/skills/wshobson)](https://smithery.ai/skills?ns=wshobson&utm_source=github&utm_medium=badge) [![Gemini CLI](https://img.shields.io/badge/Gemini%20CLI-supported-blue)](GEMINI.md)

> **🎯 代理技能已启用** — 153 项专业技能通过渐进式披露扩展了 Claude 在插件中的功能

一个全面的生产就绪系统，结合了 **185 个专业 AI 代理**、**16 个多代理工作流编排器**、**153 个代理技能**和 **100 个命令**，组织成 **80 个专注的单一用途插件**，适用于 [Claude Code](https://docs.claude.com/en/docs/claude-code/overview)。

> [!NOTE]
> **Gemini CLI 用户：** 该生态系统也以原生 Gemini CLI 扩展形式提供 — 153 个技能可按需发现，无需安装插件。详见 [GEMINI.md](GEMINI.md)。

## 概述

这个统一的代码库提供了现代软件开发中智能自动化和多代理编排所需的一切：

- **80 个专注插件** - 粒度精细、单一用途的插件，针对最小化 token 使用和可组合性进行了优化
- **185 个专业代理** - 在架构、语言、基础设施、质量、数据/AI、文档、业务运营和 SEO 方面具有深厚知识的领域专家
- **153 个代理技能** - 具有渐进式披露功能的模块化知识包，用于专业技能
- **16 个工作流编排器** - 多代理协调系统，用于复杂操作如全栈开发、安全加固、ML 管道和事件响应
- **100 个命令** - 优化的实用程序，包括项目脚手架、安全扫描、测试自动化和基础设施设置

### 主要特性

- **粒度插件架构**：80 个专注插件，针对最小化 token 使用进行了优化
- **全面工具**：100 个命令，包括测试生成、脚手架和安全扫描
- **100% 代理覆盖**：所有插件都包含专业代理
- **代理技能**：153 个专业技能，遵循渐进式披露和 token 效率原则
- **清晰组织**：25 个类别，每个类别包含 1-10 个插件，便于发现
- **高效设计**：平均每个插件 3.6 个组件（遵循 Anthropic 的 2-8 模式）

### 工作原理

每个插件都是完全独立的，拥有自己的代理、命令和技能：

- **只安装你需要的** - 每个插件只加载其特定的代理、命令和技能
- **最小化 token 使用** - 上下文中不会加载不必要的资源
- **混合搭配** - 组合多个插件用于复杂工作流
- **清晰边界** - 每个插件都有单一、专注的目的
- **渐进式披露** - 技能只在激活时加载知识

**示例**：安装 `python-development` 会加载 3 个 Python 代理、1 个脚手架工具，并提供 16 个技能（约 1000 个 token），而不是整个市场。

## 快速开始

### 第 1 步：添加市场

将此市场添加到 Claude Code：

```bash
/plugin marketplace add wshobson/agents
```

这使得所有 80 个插件都可用于安装，但**不会将任何代理或工具加载到你的上下文中**。

### 第 2 步：安装插件

浏览可用插件：

```bash
/plugin
```

安装你需要的插件：

```bash
# 基本开发插件
/plugin install python-development          # Python，包含 16 个专业技能
/plugin install javascript-typescript       # JS/TS，包含 4 个专业技能
/plugin install backend-development         # 后端 API，包含 3 个架构技能

# 基础设施和运维
/plugin install kubernetes-operations       # K8s，包含 4 个部署技能
/plugin install cloud-infrastructure        # AWS/Azure/GCP，包含 4 个云技能

# 安全和质量
/plugin install security-scanning           # SAST，包含安全技能
/plugin install comprehensive-review       # 多视角代码分析

# 全栈编排
/plugin install full-stack-orchestration   # 多代理工作流
```

每个已安装的插件**只将其特定的代理、命令和技能**加载到 Claude 的上下文中。

### 插件与代理的区别

你安装**插件**，插件中包含代理：

| 插件 | 代理 |
| ---- | ---- |
| `comprehensive-review` | architect-review, code-reviewer, security-auditor |
| `javascript-typescript` | javascript-pro, typescript-pro |
| `python-development` | python-pro, django-pro, fastapi-pro |
| `blockchain-web3` | blockchain-developer |

```bash
# ❌ 错误 - 不能直接安装代理
/plugin install typescript-pro

# ✅ 正确 - 安装插件
/plugin install javascript-typescript@claude-code-workflows
```

### 故障排除

**"找不到插件"** → 使用插件名称，而不是代理名称。添加 `@claude-code-workflows` 后缀。

**插件未加载** → 清除缓存并重新安装：

```bash
rm -rf ~/.claude/plugins/cache/claude-code-workflows && rm ~/.claude/plugins/installed_plugins.json
```

## 文档

### 核心指南

- **[插件参考](docs/plugins-zh.md)** - 所有 80 个插件的完整目录
- **[代理参考](docs/agents-zh.md)** - 按类别组织的所有 185 个代理
- **[代理技能](docs/agent-skills-zh.md)** - 153 个专业技能，支持渐进式披露
- **[使用指南](docs/usage-zh.md)** - 命令、工作流和最佳实践
- **[架构](docs/architecture-zh.md)** - 设计原则和模式
- **[PluginEval](docs/plugin-eval.md)** - 质量评估框架（层次、维度、评分）

### 快速链接

- [安装](#快速开始) - 2 步快速开始
- [基本插件](docs/plugins-zh.md#快速开始---必备插件) - 立即提高生产力的顶级插件
- [命令参考](docs/usage-zh.md#按类别分类的命令参考) - 按类别组织的所有斜杠命令
- [多代理工作流](docs/usage-zh.md#多代理工作流示例) - 预配置的编排示例
- [模型配置](docs/agents-zh.md#模型配置) - Haiku/Sonnet 混合编排

## 新功能

### Gemini CLI 扩展支持（新增）

完整的技能生态系统现在可以作为原生 Gemini CLI 扩展使用：

```bash
gemini extensions install https://github.com/wshobson/agents
```

- **153 个技能**可按需发现 — 描述你的任务，Gemini CLI 会识别匹配的技能
- **可选斜杠命令** — 通过 `make generate-plugin PLUGIN=<name>` 在本地按插件生成，不会提交到仓库
- **零变更** - 对现有代理、技能或命令无任何改动 — 所有 Markdown 文件均为平台无关格式
- **协议编排器** — 斜杠命令在检查点暂停等待用户批准，与 Claude Code 保持相同的规范流程

[→ 查看 Gemini CLI 设置和使用说明](GEMINI.md)

### PluginEval — 质量评估框架（新增）

一个三层评估框架，用于测量和认证插件/技能质量：

```bash
/plugin install plugin-eval@claude-code-workflows
```

- **三个评估层** — 静态分析（即时）、LLM 评判（语义）、蒙特卡洛模拟（统计）
- **10 个质量维度** — 触发准确性、编排适配性、输出质量、范围校准、渐进式披露、token 效率、健壮性、结构完整性、代码模板质量、生态系统一致性
- **质量徽章** — 铂金（★★★★★）、金（★★★★）、银（★★★）、铜（★★）
- **反模式检测** — OVER_CONSTRAINED、EMPTY_DESCRIPTION、MISSING_TRIGGER、BLOATED_SKILL、ORPHAN_REFERENCE、DEAD_CROSS_REF
- **统计严谨性** — Wilson 分数置信区间、Bootstrap 置信区间、Clopper-Pearson 精确置信区间、Elo 排名
- **CLI + Claude Code** — `uv run plugin-eval score/certify/compare` 或 `/eval`、`/certify`、`/compare` 命令
- **CI 门控** — `--threshold` 标志在低于最低分数时以非零退出

```bash
# 快速评估（仅静态，即时）
uv run plugin-eval score path/to/skill --depth quick

# 标准评估（静态 + LLM 评判）
uv run plugin-eval score path/to/skill --depth standard

# 完整认证（所有层次 + Elo）
uv run plugin-eval certify path/to/skill
```

[→ 查看 PluginEval 文档](docs/plugin-eval.md)

### Agent Teams 插件

使用 Claude Code 的实验性 Agent Teams 功能编排多代理团队以实现并行工作流：

```bash
/plugin install agent-teams@claude-code-workflows
```

- **7 个团队预设** — `review`、`debug`、`feature`、`fullstack`、`research`、`security`、`migration`
- **并行代码审查** — `/team-review src/ --reviewers security,performance,architecture`
- **假设驱动调试** — `/team-debug "API 返回 500" --hypotheses 3`
- **并行功能开发** — `/team-feature "添加 OAuth2 认证" --plan-first`
- **研究团队** — 跨代码库和 Web 来源的并行调查
- **安全审计** — 4 名审查员覆盖 OWASP、认证、依赖和密钥
- **迁移支持** — 并行流协调迁移并进行正确性验证

包含 4 个专业代理、7 个命令和 6 个技能，以及参考文档。

[→ 查看 agent-teams 文档](plugins/agent-teams/README.md)

### Conductor 插件 — 上下文驱动开发

将 Claude Code 转变为项目管理工具，采用结构化的**上下文 → 规范与计划 → 实现**工作流：

```bash
/plugin install conductor@claude-code-workflows
```

- **交互式设置** — `/conductor:setup` 创建产品愿景、技术栈、工作流规则和风格指南
- **基于轨道的开发** — `/conductor:new-track` 生成规范和分阶段实现计划
- **TDD 工作流** — `/conductor:implement` 以验证检查点执行任务
- **语义回滚** — `/conductor:revert` 按逻辑单元（轨道、阶段或任务）撤销工作
- **状态持久化** — 跨会话恢复设置，持久化项目上下文
- **3 个技能** — 上下文驱动开发、轨道管理、工作流模式

[→ 查看 Conductor 文档](plugins/conductor/README.md)

### 代理技能（40 个插件中的 153 个技能）

遵循 Anthropic 渐进式披露架构的专业知识包：

**语言开发：**

- **Python**（5 个技能）：异步模式、测试、打包、性能、UV 包管理器
- **JavaScript/TypeScript**（4 个技能）：高级类型、Node.js 模式、测试、现代 ES6+

**基础设施和 DevOps：**

- **Kubernetes**（4 个技能）：清单、Helm 图表、GitOps、安全策略
- **云基础设施**（4 个技能）：Terraform、多云、混合网络、成本优化
- **CI/CD**（4 个技能）：管道设计、GitHub Actions、GitLab CI、密钥管理

**开发和架构：**

- **后端**（3 个技能）：API 设计、架构模式、微服务
- **LLM 应用程序**（8 个技能）：LangGraph、提示工程、RAG、评估、嵌入、相似性搜索、向量调优、混合搜索

**区块链和 Web3**（4 个技能）：DeFi 协议、NFT 标准、Solidity 安全、Web3 测试

**项目管理：**

- **Conductor**（3 个技能）：上下文驱动开发、轨道管理、工作流模式

**还有更多：** 框架迁移、可观测性、支付处理、ML 操作、安全扫描

[→ 查看完整技能文档](docs/agent-skills-zh.md)

### 三层模型策略

战略性模型分配以实现最佳性能和成本：

| 层级 | 模型 | 代理数量 | 使用场景 |
| ---- | ---- | -------- | -------- |
| **Tier 1** | Opus 4.7 | 42 | 关键架构、安全、所有代码审查、生产编码（语言专家、框架） |
| **Tier 2** | Inherit | 42 | 复杂任务 - 用户在运行时选择模型（AI/ML、后端、前端/移动、专业化） |
| **Tier 3** | Sonnet | 51 | 智能支持（文档、测试、调试、网络、API 文档、DX、遗留、支付） |
| **Tier 4** | Haiku | 18 | 快速运维任务（SEO、部署、简单文档、销售、内容、搜索） |

**为什么关键代理使用 Opus 4.7？**

- SWE-bench 得分 80.8%（业界领先）
- 复杂任务减少 65% 的 token 使用
- 最适合架构决策和安全审计

**Tier 2 灵活性（`inherit`）：**
标记为 `inherit` 的代理使用你当前会话的默认模型，让你可以平衡成本和能力：

- 通过 `claude --model opus` 或 `claude --model sonnet` 启动会话时设置
- 如果未指定默认值，则回退到 Sonnet 4.6
- 非常适合希望控制成本的前端/移动开发者
- AI/ML 工程师可以为复杂模型工作选择 Opus

**成本考虑：**

- **Opus 4.7**：每百万输入/输出 token $5/$25 — 关键工作的高端选择
- **Sonnet 4.6**：每百万 token $3/$15 — 均衡的性能/成本
- **Haiku 4.5**：每百万 token $1/$5 — 快速、经济高效的运维
- Opus 在复杂任务上减少 65% 的 token 使用，往往能抵消更高的费率
- 使用 `inherit` 层控制高频使用场景的成本

编排模式结合模型以提高效率：

```
Opus（架构）→ Sonnet（开发）→ Haiku（部署）
```

[→ 查看模型配置详情](docs/agents-zh.md#模型配置)

## 热门用例

### 全栈功能开发

```bash
/full-stack-orchestration:full-stack-feature "使用 OAuth2 的用户认证"
```

协调 7 个以上代理：backend-architect → database-architect → frontend-developer → test-automator → security-auditor → deployment-engineer → observability-engineer

[→ 查看所有工作流示例](docs/usage-zh.md#多代理工作流示例)

### 安全加固

```bash
/security-scanning:security-hardening --level comprehensive
```

多代理安全评估，包括 SAST、依赖扫描和代码审查。

### 使用现代工具的 Python 开发

```bash
/python-development:python-scaffold fastapi-microservice
```

创建生产就绪的 FastAPI 项目，具有异步模式，激活技能：

- `async-python-patterns` - AsyncIO 和并发
- `python-testing-patterns` - pytest 和 fixtures
- `uv-package-manager` - 快速依赖管理

### Kubernetes 部署

```bash
# 自动激活 k8s 技能
"创建带 Helm 图表和 GitOps 的生产 Kubernetes 部署"
```

使用 kubernetes-architect 代理和 4 个专业技能创建生产级配置。

[→ 查看完整使用指南](docs/usage-zh.md)

## 插件类别

**25 个类别，80 个插件：**

- 🎨 **开发**（6 个）- 调试、后端、前端、多平台
- 📚 **文档**（4 个）- 代码文档、API 规范、图表、C4 架构、**HADS**（人机文档标准）
- 🔄 **工作流**（5 个）- git、全栈、TDD、**Conductor**（上下文驱动开发）、**Agent Teams**（多代理编排）
- ✅ **测试**（2 个）- 单元测试、**qa-orchestra**（多代理 QA 工具包，含 Chrome MCP 验证）
- 🔍 **质量**（3 个）- 综合审查、性能
- 🤖 **AI & ML**（4 个）- LLM 应用、代理编排、上下文、MLOps
- 📊 **数据**（2 个）- 数据工程、数据验证
- 🗄️ **数据库**（2 个）- 数据库设计、迁移
- 🚨 **运维**（4 个）- 事件响应、诊断、分布式调试、可观测性
- ⚡ **性能**（2 个）- 应用性能、数据库/云优化
- ☁️ **基础设施**（5 个）- 部署、验证、Kubernetes、云、CI/CD
- 🔒 **安全**（6 个）- 扫描、合规、后端/API、前端/移动、**block-no-verify**（git hook 绕过防护）
- 🛡️ **治理**（1 个）- **protect-mcp**（Cedar 策略执行 + Ed25519 签名收据）
- 💻 **语言**（10 个）- Python、JS/TS、系统、JVM、脚本、函数式、嵌入式
- 🔗 **区块链**（1 个）- 智能合约、DeFi、Web3
- 💰 **金融**（1 个）- 量化交易、风险管理
- 💳 **支付**（1 个）- Stripe、PayPal、账单
- 🎮 **游戏**（1 个）- Unity、Minecraft 插件
- 🎨 **创意**（1 个）- 创意工具
- ♿ **无障碍**（1 个）- WCAG 和 a11y
- 📢 **营销**（4 个）- SEO 内容、技术 SEO、SEO 分析、内容营销
- 💼 **业务**（4 个）- 分析、HR/法律、客户/销售
- 🔌 **API**（2 个）- API 工具
- 🛠️ **实用工具**（4 个）- 通用辅助工具
- 🔧 **现代化**（2 个）- 遗留迁移和重构

[→ 查看完整插件目录](docs/plugins-zh.md)

### 相关插件

托管在各自市场的插件 — 从源头安装以获取最新版本：

- **[Pensyve](https://github.com/major7apps/pensyve)** — 为 Claude Code 提供跨会话认知记忆的通用记忆运行时。智能捕获、实体感知召回、6 个命令、4 个技能、2 个代理和 6 个生命周期 hook。

  ```bash
  /plugin marketplace add major7apps/pensyve
  /plugin install pensyve@major7apps-pensyve
  ```

## 架构亮点

### 粒度设计

- **单一职责** - 每个插件都专注于做好一件事
- **最小化 token 使用** - 平均每个插件 3.6 个组件
- **可组合** - 混合搭配用于复杂工作流
- **100% 覆盖** - 所有 185 个代理都可通过插件访问

### 渐进式披露（技能）

三层架构以实现 token 效率：

1. **元数据** - 名称和激活条件（始终加载）
2. **指令** - 核心指导（激活时加载）
3. **资源** - 示例和模板（按需加载）

### 代码库结构

```
claude-agents/
├── .claude-plugin/
│   └── marketplace.json          # 81 个插件（80 本地 + 1 外部）
├── plugins/
│   ├── python-development/
│   │   ├── agents/               # 3 个 Python 专家
│   │   ├── commands/             # 脚手架工具
│   │   └── skills/               # 5 个专业技能
│   ├── kubernetes-operations/
│   │   ├── agents/               # K8s 架构师
│   │   ├── commands/             # 部署工具
│   │   └── skills/               # 4 个 K8s 技能
│   └── ... (还有 67 个插件)
├── docs/                          # 全面文档
└── README.md                      # 此文件
```

[→ 查看架构详情](docs/architecture-zh.md)

## 贡献

要添加新代理、技能或命令：

1. 在 `plugins/` 中识别或创建适当的插件目录
2. 在适当的子目录中创建 `.md` 文件：
   - `agents/` - 用于专业代理
   - `commands/` - 用于工具和工作流
   - `skills/` - 用于模块化知识包
3. 遵循命名约定（小写，连字符分隔）
4. 编写清晰的激活条件和全面的内容
5. 更新 `.claude-plugin/marketplace.json` 中的插件定义

请参阅[架构文档](docs/architecture-zh.md)获取详细指南。

## 资源

### 文档

- [Claude Code 文档](https://docs.claude.com/en/docs/claude-code/overview)
- [插件指南](https://docs.claude.com/en/docs/claude-code/plugins)
- [子代理指南](https://docs.claude.com/en/docs/claude-code/sub-agents)
- [代理技能指南](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
- [斜杠命令参考](https://docs.claude.com/en/docs/claude-code/slash-commands)

### 此代码库

- [插件参考](docs/plugins-zh.md)
- [代理参考](docs/agents-zh.md)
- [代理技能指南](docs/agent-skills-zh.md)
- [使用指南](docs/usage-zh.md)
- [架构](docs/architecture-zh.md)

## 许可证

MIT 许可证 - 详情请见 [LICENSE](LICENSE) 文件。

## 星标历史

[![星标历史图表](https://api.star-history.com/svg?repos=wshobson/agents&type=date&legend=top-left)](https://www.star-history.com/#wshobson/agents&type=date&legend=top-left)

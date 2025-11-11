**中文** | [English](README.md)

## 新增插件

- **[领域驱动设计（DDD）](plugins/domain-driven-design/README.md)** - 战术和战略设计模式、限界上下文、统一语言
- **[四加一视图架构](plugins/four-plus-one-architecture/README.md)** - 用例视图、逻辑视图、流程视图、物理视图、场景视图
- **[现代企业架构框架（MEAF）](plugins/modern-enterprise-architecture/README.md)** - 企业架构规划、技术选型、系统演进

---

# Claude Code 插件：编排与自动化

> **⚡ 为Sonnet 4.5和Haiku 4.5更新** — 所有代理已针对最新模型进行优化，支持混合编排
>
> **🎯 代理技能启用** — 47项专业技能扩展了Claude在插件中的功能，支持渐进式披露

一个全面的生产就绪系统，结合了**85个专业AI代理**、**15个多代理工作流编排器**、**47个代理技能**和**44个开发工具**，组织成**63个专注的单一用途插件**，适用于[Claude Code](https://docs.claude.com/en/docs/claude-code/overview)。

## 概述

这个统一的代码库提供了现代软件开发中智能自动化和多代理编排所需的一切：

- **63个专注插件** - 粒度精细、单一用途的插件，针对最小化令牌使用和可组合性进行了优化
- **85个专业代理** - 在架构、语言、基础设施、质量、数据/AI、文档、业务运营和SEO方面具有深厚知识的领域专家
- **47个代理技能** - 具有渐进式披露功能的模块化知识包，用于专业技能
- **15个工作流编排器** - 多代理协调系统，用于复杂操作如全栈开发、安全加固、ML管道和事件响应
- **44个开发工具** - 优化的实用程序，包括项目脚手架、安全扫描、测试自动化和基础设施设置

### 主要特性

- **粒度插件架构**：63个专注插件，针对最小化令牌使用进行了优化
- **全面工具**：44个开发工具，包括测试生成、脚手架和安全扫描
- **100%代理覆盖**：所有插件都包含专业代理
- **代理技能**：47个专业技能，遵循渐进式披露和令牌效率原则
- **清晰组织**：23个类别，每个类别包含1-6个插件，便于发现
- **高效设计**：平均每个插件3.4个组件（遵循Anthropic的2-8模式）

### 工作原理

每个插件都是完全独立的，拥有自己的代理、命令和技能：

- **只安装你需要的** - 每个插件只加载其特定的代理、命令和技能
- **最小化令牌使用** - 上下文中不会加载不必要的资源
- **混合搭配** - 组合多个插件用于复杂工作流
- **清晰边界** - 每个插件都有单一、专注的目的
- **渐进式披露** - 技能只在激活时加载知识

**示例**：安装`python-development`会加载3个Python代理、1个脚手架工具，并提供5个技能（约300个令牌），而不是整个市场。

## 快速开始

### 第1步：添加市场

将此市场添加到Claude Code：

```bash
/plugin marketplace add wshobson/agents
```

这使得所有63个插件都可用于安装，但**不会将任何代理或工具加载到你的上下文中**。

### 第2步：安装插件

浏览可用插件：

```bash
/plugin
```

安装你需要的插件：

```bash
# 基本开发插件
/plugin install python-development          # Python，包含5个专业技能
/plugin install javascript-typescript       # JS/TS，包含4个专业技能
/plugin install backend-development         # 后端API，包含3个架构技能

# 基础设施和运维
/plugin install kubernetes-operations       # K8s，包含4个部署技能
/plugin install cloud-infrastructure        # AWS/Azure/GCP，包含4个云技能

# 安全和质量
/plugin install security-scanning           # SAST，包含安全技能
/plugin install code-review-ai             # AI驱动的代码审查

# 全栈编排
/plugin install full-stack-orchestration   # 多代理工作流
```

每个已安装的插件**只将其特定的代理、命令和技能**加载到Claude的上下文中。

## 文档

### 核心指南

- **[插件参考](docs/plugins-zh.md)** - 所有63个插件的完整目录
- **[代理参考](docs/agents-zh.md)** - 按类别组织的所有85个代理
- **[代理技能](docs/agent-skills-zh.md)** - 47个专业技能，支持渐进式披露
- **[使用指南](docs/usage-zh.md)** - 命令、工作流和最佳实践
- **[架构](docs/architecture-zh.md)** - 设计原则和模式

### 快速链接

- [安装](#快速开始) - 2步快速开始
- [基本插件](docs/plugins-zh.md#快速开始---基本插件) - 立即提高生产力的顶级插件
- [命令参考](docs/usage-zh.md#按类别组织的命令参考) - 按类别组织的所有斜杠命令
- [多代理工作流](docs/usage-zh.md#多代理工作流示例) - 预配置的编排示例
- [模型配置](docs/agents-zh.md#模型配置) - Haiku/Sonnet混合编排

## 新功能

### 代理技能（14个插件中的47个技能）

遵循Anthropic渐进式披露架构的专业知识包：

**语言开发：**
- **Python**（5个技能）：异步模式、测试、打包、性能、UV包管理器
- **JavaScript/TypeScript**（4个技能）：高级类型、Node.js模式、测试、现代ES6+

**基础设施和DevOps：**
- **Kubernetes**（4个技能）：清单、Helm图表、GitOps、安全策略
- **云基础设施**（4个技能）：Terraform、多云、混合网络、成本优化
- **CI/CD**（4个技能）：管道设计、GitHub Actions、GitLab CI、密钥管理

**开发和架构：**
- **后端**（3个技能）：API设计、架构模式、微服务
- **LLM应用程序**（4个技能）：LangChain、提示工程、RAG、评估

**区块链和Web3**（4个技能）：DeFi协议、NFT标准、Solidity安全、Web3测试

**还有更多：** 框架迁移、可观察性、支付处理、ML操作、安全扫描

[→ 查看完整技能文档](docs/agent-skills-zh.md)

### 混合模型编排

战略性模型分配以实现最佳性能和成本：
- **47个Haiku代理** - 快速执行确定性任务
- **97个Sonnet代理** - 复杂推理和架构

编排模式结合模型以提高效率：
```
Sonnet（规划）→ Haiku（执行）→ Sonnet（审查）
```

[→ 查看模型配置详情](docs/agents-zh.md#模型配置)

## 热门用例

### 全栈功能开发

```bash
/full-stack-orchestration:full-stack-feature "使用OAuth2的用户认证"
```

协调7个以上代理：后端架构师 → 数据库架构师 → 前端开发者 → 测试自动化 → 安全审计师 → 部署工程师 → 可观察性工程师

[→ 查看所有工作流示例](docs/usage-zh.md#多代理工作流示例)

### 安全加固

```bash
/security-scanning:security-hardening --level comprehensive
```

多代理安全评估，包括SAST、依赖扫描和代码审查。

### 使用现代工具的Python开发

```bash
/python-development:python-scaffold fastapi-microservice
```

创建生产就绪的FastAPI项目，具有异步模式，激活技能：
- `async-python-patterns` - AsyncIO和并发
- `python-testing-patterns` - pytest和fixtures
- `uv-package-manager` - 快速依赖管理

### Kubernetes部署

```bash
# 自动激活k8s技能
"创建带Helm图表和GitOps的生产Kubernetes部署"
```

使用kubernetes-architect代理和4个专业技能创建生产级配置。

[→ 查看完整使用指南](docs/usage-zh.md)

## 插件类别

**23个类别，63个插件：**

- 🎨 **开发**（4个） - 调试、后端、前端、多平台
- 📚 **文档**（2个） - 代码文档、API规范、图表
- 🔄 **工作流**（3个） - git、全栈、TDD
- ✅ **测试**（2个） - 单元测试、TDD工作流
- 🔍 **质量**（3个） - 代码审查、综合审查、性能
- 🤖 **AI和ML**（4个） - LLM应用、代理编排、上下文、MLOps
- 📊 **数据**（2个） - 数据工程、数据验证
- 🗄️ **数据库**（2个） - 数据库设计、迁移
- 🚨 **运维**（4个） - 事件响应、诊断、分布式调试、可观察性
- ⚡ **性能**（2个） - 应用性能、数据库/云优化
- ☁️ **基础设施**（5个） - 部署、验证、Kubernetes、云、CI/CD
- 🔒 **安全**（4个） - 扫描、合规、后端/API、前端/移动
- 💻 **语言**（7个） - Python、JS/TS、系统、JVM、脚本、函数式、嵌入式
- 🔗 **区块链**（1个） - 智能合约、DeFi、Web3
- 💰 **金融**（1个） - 量化交易、风险管理
- 💳 **支付**（1个） - Stripe、PayPal、账单
- 🎮 **游戏**（1个） - Unity、Minecraft插件
- 📢 **营销**（4个） - SEO内容、技术SEO、SEO分析、内容营销
- 💼 **业务**（3个） - 分析、HR/法律、客户/销售
- 还有更多...

[→ 查看完整插件目录](docs/plugins-zh.md)

## 架构亮点

### 粒度设计

- **单一职责** - 每个插件都专注于做好一件事
- **最小化令牌使用** - 平均每个插件3.4个组件
- **可组合** - 混合搭配用于复杂工作流
- **100%覆盖** - 所有85个代理都可通过插件访问

### 渐进式披露（技能）

三层架构以实现令牌效率：
1. **元数据** - 名称和激活条件（始终加载）
2. **指令** - 核心指导（激活时加载）
3. **资源** - 示例和模板（按需加载）

### 代码库结构

```
claude-agents/
├── .claude-plugin/
│   └── marketplace.json          # 63个插件
├── plugins/
│   ├── python-development/
│   │   ├── agents/               # 3个Python专家
│   │   ├── commands/             # 脚手架工具
│   │   └── skills/               # 5个专业技能
│   ├── kubernetes-operations/
│   │   ├── agents/               # K8s架构师
│   │   ├── commands/             # 部署工具
│   │   └── skills/               # 4个K8s技能
│   └── ... (还有61个插件)
├── docs/                          # 全面文档
└── README.md                      # 此文件
```

[→ 查看架构详情](docs/architecture-zh.md)

## 贡献

要添加新代理、技能或命令：

1. 在`plugins/`中识别或创建适当的插件目录
2. 在适当的子目录中创建`.md`文件：
   - `agents/` - 用于专业代理
   - `commands/` - 用于工具和工作流
   - `skills/` - 用于模块化知识包
3. 遵循命名约定（小写，连字符分隔）
4. 编写清晰的激活条件和全面的内容
5. 更新`.claude-plugin/marketplace.json`中的插件定义

请参阅[架构文档](docs/architecture-zh.md)获取详细指南。

## 资源

### 文档
- [Claude Code文档](https://docs.claude.com/en/docs/claude-code/overview)
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

MIT许可证 - 详情请见[LICENSE](LICENSE)文件。

## 星标历史

[![星标历史图表](https://api.star-history.com/svg?repos=wshobson/agents&type=date&legend=top-left)](https://www.star-history.com/#wshobson/agents&type=date&legend=top-left)

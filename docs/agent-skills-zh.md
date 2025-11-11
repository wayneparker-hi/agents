## Agent Skills

Agent Skills是模块化包，通过专门的领域知识扩展Claude的功能，遵循Anthropic的[Agent Skills规范](https://github.com/anthropics/skills/blob/main/agent_skills_spec.md)。这个插件生态系统包括**55个专业技能**，跨越15个插件，实现渐进式披露和高效的token使用。

### 概述

Skills为Claude提供特定领域的深度专业知识，而无需预先将所有内容加载到上下文中。每个skill包括：

- **YAML Frontmatter**：名称和激活条件
- **渐进式披露**：元数据→指令→资源
- **激活触发器**：明确的"使用时机"子句，用于自动调用

### 按插件分类的Skills

#### Kubernetes Operations (4个技能)

| Skill | 描述 |
|-------|------|
| **k8s-manifest-generator** | 创建生产就绪的Kubernetes清单，包括Deployments、Services、ConfigMaps和Secrets，遵循最佳实践 |
| **helm-chart-scaffolding** | 设计、组织和管理Helm图表，用于模板化和打包Kubernetes应用程序 |
| **gitops-workflow** | 使用ArgoCD和Flux实现GitOps工作流，用于自动化、声明式部署 |
| **k8s-security-policies** | 实现Kubernetes安全策略，包括NetworkPolicy、PodSecurityPolicy和RBAC |

#### LLM Application Development (4个技能)

| Skill | 描述 |
|-------|------|
| **langchain-architecture** | 使用LangChain框架设计LLM应用程序，包括agents、内存和工具集成 |
| **prompt-engineering-patterns** | 掌握高级提示工程技术，以提高LLM的性能和可靠性 |
| **rag-implementation** | 构建检索增强生成系统，使用向量数据库和语义搜索 |
| **llm-evaluation** | 实现全面的评估策略，包括自动化指标和基准测试 |

#### Backend Development (3个技能)

| Skill | 描述 |
|-------|------|
| **api-design-principles** | 掌握REST和GraphQL API设计，实现直观、可扩展和可维护的API |
| **architecture-patterns** | 实现清洁架构、六边形架构和领域驱动设计 |
| **microservices-patterns** | 设计微服务，包括服务边界、事件驱动通信和弹性 |

#### Developer Essentials (8个技能)

| Skill | 描述 |
|-------|------|
| **git-advanced-workflows** | 掌握高级Git工作流，包括rebase、cherry-pick、bisect、worktrees和reflog |
| **sql-optimization-patterns** | 优化SQL查询、索引策略和EXPLAIN分析，以提高数据库性能 |
| **error-handling-patterns** | 实现健壮的错误处理，包括异常、Result类型和优雅降级 |
| **code-review-excellence** | 提供有效的代码审查，包括建设性反馈和系统分析 |
| **e2e-testing-patterns** | 使用Playwright和Cypress构建可靠的端到端测试套件，用于关键用户工作流 |
| **auth-implementation-patterns** | 实现身份验证和授权，包括JWT、OAuth2、会话和RBAC |
| **debugging-strategies** | 掌握系统调试技术、分析工具和根本原因分析 |
| **monorepo-management** | 使用Turborepo、Nx和pnpm工作区管理monorepo，用于可扩展的多包项目 |

#### Blockchain & Web3 (4个技能)

| Skill | 描述 |
|-------|------|
| **defi-protocol-templates** | 实现DeFi协议，包括质押、AMMs、治理和借贷的模板 |
| **nft-standards** | 实现NFT标准(ERC-721、ERC-1155)，包括元数据和市场集成 |
| **solidity-security** | 掌握智能合约安全，防止漏洞并实现安全模式 |
| **web3-testing** | 使用Hardhat和Foundry测试智能合约，包括单元测试和主网分叉 |

#### CI/CD Automation (4个技能)

| Skill | 描述 |
|-------|------|
| **deployment-pipeline-design** | 设计多阶段CI/CD管道，包括审批门和安全检查 |
| **github-actions-templates** | 创建生产就绪的GitHub Actions工作流，用于测试、构建和部署 |
| **gitlab-ci-patterns** | 构建GitLab CI/CD管道，包括多阶段工作流和分布式运行器 |
| **secrets-management** | 实现安全的秘密管理，使用Vault、AWS Secrets Manager或原生解决方案 |

#### Cloud Infrastructure (4个技能)

| Skill | 描述 |
|-------|------|
| **terraform-module-library** | 构建可重用的Terraform模块，用于AWS、Azure和GCP基础设施 |
| **multi-cloud-architecture** | 设计多云架构，避免供应商锁定 |
| **hybrid-cloud-networking** | 配置本地和云平台之间的安全连接 |
| **cost-optimization** | 通过正确的大小调整、标记和预留实例优化云成本 |

#### Framework Migration (4个技能)

| Skill | 描述 |
|-------|------|
| **react-modernization** | 升级React应用，迁移到hooks，并采用并发特性 |
| **angular-migration** | 使用混合模式和增量重写从AngularJS迁移到Angular |
| **database-migration** | 执行数据库迁移，采用零停机策略和转换 |
| **dependency-upgrade** | 管理主要依赖升级，包括兼容性分析和测试 |

#### Observability & Monitoring (4个技能)

| Skill | 描述 |
|-------|------|
| **prometheus-configuration** | 设置Prometheus，用于全面的指标收集和监控 |
| **grafana-dashboards** | 创建生产Grafana仪表板，用于实时系统可视化 |
| **distributed-tracing** | 实现分布式跟踪，使用Jaeger和Tempo跟踪请求 |
| **slo-implementation** | 定义SLIs和SLOs，包括错误预算和告警 |

#### Payment Processing (4个技能)

| Skill | 描述 |
|-------|------|
| **stripe-integration** | 实现Stripe支付处理，包括结账、订阅和webhooks |
| **paypal-integration** | 集成PayPal支付处理，包括快速结账和订阅 |
| **pci-compliance** | 实现PCI DSS合规，用于安全支付卡数据处理 |
| **billing-automation** | 构建自动化计费系统，用于经常性支付和发票 |

#### Python Development (5个技能)

| Skill | 描述 |
|-------|------|
| **async-python-patterns** | 掌握Python asyncio、并发编程和async/await模式 |
| **python-testing-patterns** | 实现全面的测试，包括pytest、fixtures和mocking |
| **python-packaging** | 创建可分发的Python包，包括适当的结构和PyPI发布 |
| **python-performance-optimization** | 使用cProfile和性能最佳实践分析和优化Python代码 |
| **uv-package-manager** | 掌握uv包管理器，用于快速依赖管理和虚拟环境 |

#### JavaScript/TypeScript (4个技能)

| Skill | 描述 |
|-------|------|
| **typescript-advanced-types** | 掌握TypeScript的高级类型系统，包括泛型和条件类型 |
| **nodejs-backend-patterns** | 构建生产就绪的Node.js服务，包括Express/Fastify和最佳实践 |
| **javascript-testing-patterns** | 实现全面的测试，包括Jest、Vitest和Testing Library |
| **modern-javascript-patterns** | 掌握ES6+特性，包括async/await、解构和函数式编程 |

#### API Scaffolding (1个技能)

| Skill | 描述 |
|-------|------|
| **fastapi-templates** | 创建生产就绪的FastAPI项目，包括异步模式和错误处理 |

#### Machine Learning Operations (1个技能)

| Skill | 描述 |
|-------|------|
| **ml-pipeline-workflow** | 构建端到端的MLOps管道，从数据准备到部署 |

#### Security Scanning (1个技能)

| Skill | 描述 |
|-------|------|
| **sast-configuration** | 配置静态应用程序安全测试工具，用于漏洞检测 |

### Skills如何工作

#### 激活

当Claude在您的请求中检测到匹配模式时，Skills会自动激活：

```
用户："设置Kubernetes部署与Helm图表"
→ 激活：helm-chart-scaffolding、k8s-manifest-generator

用户："构建用于文档问答的RAG系统"
→ 激活：rag-implementation、prompt-engineering-patterns

用户："优化Python异步性能"
→ 激活：async-python-patterns、python-performance-optimization
```

#### 渐进式披露

Skills使用三层架构实现token效率：

1. **元数据** (Frontmatter)：名称和激活条件（始终加载）
2. **指令**：核心指导和模式（激活时加载）
3. **资源**：示例和模板（按需加载）

#### 与Agents的集成

Skills与agents协同工作，提供深度领域专业知识：

- **Agents**：高级推理和编排
- **Skills**：专业化的知识和实现模式

示例工作流：
```
backend-architect agent → 计划API架构
  ↓
api-design-principles skill → 提供REST/GraphQL最佳实践
  ↓
fastapi-templates skill → 提供生产就绪模板
```

### 规范合规性

所有55个skills都遵循[Agent Skills规范](https://github.com/anthropics/skills/blob/main/agent_skills_spec.md)：

- ✓ 必需的`name`字段（连字符命名）
- ✓ 必需的`description`字段，带有"Use when"子句
- ✓ 描述少于1024个字符
- ✓ 完整、非截断的描述
- ✓ 正确的YAML frontmatter格式

### 创建新Skills

要向插件添加skill：

1. 创建`plugins/{plugin-name}/skills/{skill-name}/SKILL.md`
2. 添加YAML frontmatter：
   ```yaml
   ---
   name: skill-name
   description: skill的作用。使用时机[激活触发器]。
   ---
   ```
3. 使用渐进式披露编写全面的skill内容
4. 将skill路径添加到`marketplace.json`：
   ```json
   {
     "name": "plugin-name",
     "skills": ["./skills/skill-name"]
   }
   ```

#### Skill结构

```
plugins/{plugin-name}/
└── skills/
    └── {skill-name}/
        └── SKILL.md        # Frontmatter + 内容
```

### 优势

- **Token效率**：仅在需要时加载相关知识
- **专业化的专业知识**：深度领域知识而无冗余
- **清晰激活**：明确的触发器防止不必要的调用
- **可组合性**：跨工作流混合和匹配skills
- **可维护性**：隔离更新不影响其他skills

### 资源

- [Anthropic Skills仓库](https://github.com/anthropics/skills)
- [Agent Skills文档](https://docs.claude.com/en/docs/claude-code/skills)

---

## Agent Reference

所有**85个专业AI agents**的完整参考，按类别组织并分配模型。

### Agent类别

#### 架构与系统设计

##### 核心架构

| Agent | 模型 | 描述 |
|-------|------|------|
| [backend-architect](../plugins/backend-development/agents/backend-architect.md) | opus | RESTful API设计、微服务边界、数据库模式 |
| [frontend-developer](../plugins/multi-platform-apps/agents/frontend-developer.md) | sonnet | React组件、响应式布局、客户端状态管理 |
| [graphql-architect](../plugins/backend-development/agents/graphql-architect.md) | opus | GraphQL模式、解析器、联合架构 |
| [architect-reviewer](../plugins/comprehensive-review/agents/architect-review.md) | opus | 架构一致性分析和模式验证 |
| [cloud-architect](../plugins/cloud-infrastructure/agents/cloud-architect.md) | opus | AWS/Azure/GCP基础设施设计和成本优化 |
| [hybrid-cloud-architect](../plugins/cloud-infrastructure/agents/hybrid-cloud-architect.md) | opus | 跨云和本地环境的多云策略 |
| [kubernetes-architect](../plugins/kubernetes-operations/agents/kubernetes-architect.md) | opus | 使用Kubernetes和GitOps的云原生基础设施 |

##### UI/UX与移动

| Agent | 模型 | 描述 |
|-------|------|------|
| [ui-ux-designer](../plugins/multi-platform-apps/agents/ui-ux-designer.md) | sonnet | 界面设计、线框图、设计系统 |
| [ui-visual-validator](../plugins/accessibility-compliance/agents/ui-visual-validator.md) | sonnet | 视觉回归测试和UI验证 |
| [mobile-developer](../plugins/multi-platform-apps/agents/mobile-developer.md) | sonnet | React Native和Flutter应用程序开发 |
| [ios-developer](../plugins/multi-platform-apps/agents/ios-developer.md) | sonnet | 使用Swift/SwiftUI的原生iOS开发 |
| [flutter-expert](../plugins/multi-platform-apps/agents/flutter-expert.md) | sonnet | 高级Flutter开发与状态管理 |

#### 编程语言

##### 系统与底层

| Agent | 模型 | 描述 |
|-------|------|------|
| [c-pro](../plugins/systems-programming/agents/c-pro.md) | sonnet | 系统编程，包括内存管理和操作系统接口 |
| [cpp-pro](../plugins/systems-programming/agents/cpp-pro.md) | sonnet | 现代C++，包括RAII、智能指针、STL算法 |
| [rust-pro](../plugins/systems-programming/agents/rust-pro.md) | sonnet | 内存安全的系统编程，包括所有权模式 |
| [golang-pro](../plugins/systems-programming/agents/golang-pro.md) | sonnet | 使用goroutines和channels的并发编程 |

##### Web与应用

| Agent | 模型 | 描述 |
|-------|------|------|
| [javascript-pro](../plugins/javascript-typescript/agents/javascript-pro.md) | sonnet | 现代JavaScript，包括ES6+、异步模式、Node.js |
| [typescript-pro](../plugins/javascript-typescript/agents/typescript-pro.md) | sonnet | 高级TypeScript，包括类型系统和泛型 |
| [python-pro](../plugins/python-development/agents/python-pro.md) | sonnet | Python开发，包括高级特性和优化 |
| [ruby-pro](../plugins/web-scripting/agents/ruby-pro.md) | sonnet | Ruby，包括元编程、Rails模式、gem开发 |
| [php-pro](../plugins/web-scripting/agents/php-pro.md) | sonnet | 现代PHP，包括框架和性能优化 |

##### 企业与JVM

| Agent | 模型 | 描述 |
|-------|------|------|
| [java-pro](../plugins/jvm-languages/agents/java-pro.md) | sonnet | 现代Java，包括流、并发、JVM优化 |
| [scala-pro](../plugins/jvm-languages/agents/scala-pro.md) | sonnet | 企业级Scala，包括函数式编程和分布式系统 |
| [csharp-pro](../plugins/jvm-languages/agents/csharp-pro.md) | sonnet | C#开发，包括.NET框架和模式 |

##### 专业化平台

| Agent | 模型 | 描述 |
|-------|------|------|
| [elixir-pro](../plugins/functional-programming/agents/elixir-pro.md) | sonnet | Elixir，包括OTP模式和Phoenix框架 |
| [django-pro](../plugins/api-scaffolding/agents/django-pro.md) | sonnet | Django开发，包括ORM和异步视图 |
| [fastapi-pro](../plugins/api-scaffolding/agents/fastapi-pro.md) | sonnet | FastAPI，包括异步模式和Pydantic |
| [unity-developer](../plugins/game-development/agents/unity-developer.md) | sonnet | Unity游戏开发和优化 |
| [minecraft-bukkit-pro](../plugins/game-development/agents/minecraft-bukkit-pro.md) | sonnet | Minecraft服务器插件开发 |
| [sql-pro](../plugins/database-design/agents/sql-pro.md) | sonnet | 复杂SQL查询和数据库优化 |

#### 基础设施与运维

##### DevOps与部署

| Agent | 模型 | 描述 |
|-------|------|------|
| [devops-troubleshooter](../plugins/incident-response/agents/devops-troubleshooter.md) | sonnet | 生产调试、日志分析、部署故障排除 |
| [deployment-engineer](../plugins/cloud-infrastructure/agents/deployment-engineer.md) | sonnet | CI/CD管道、容器化、云部署 |
| [terraform-specialist](../plugins/cloud-infrastructure/agents/terraform-specialist.md) | sonnet | 基础设施即代码，包括Terraform模块和状态管理 |
| [dx-optimizer](../plugins/team-collaboration/agents/dx-optimizer.md) | sonnet | 开发者体验优化和工具改进 |

##### 数据库管理

| Agent | 模型 | 描述 |
|-------|------|------|
| [database-optimizer](../plugins/observability-monitoring/agents/database-optimizer.md) | sonnet | 查询优化、索引设计、迁移策略 |
| [database-admin](../plugins/database-migrations/agents/database-admin.md) | sonnet | 数据库操作、备份、复制、监控 |
| [database-architect](../plugins/database-design/agents/database-architect.md) | opus | 从零开始的数据库设计、技术选择、模式建模 |

##### 事件响应与网络

| Agent | 模型 | 描述 |
|-------|------|------|
| [incident-responder](../plugins/incident-response/agents/incident-responder.md) | opus | 生产事件管理和解决 |
| [network-engineer](../plugins/observability-monitoring/agents/network-engineer.md) | sonnet | 网络调试、负载均衡、流量分析 |

#### 质量保证与安全

##### 代码质量与审查

| Agent | 模型 | 描述 |
|-------|------|------|
| [code-reviewer](../plugins/comprehensive-review/agents/code-reviewer.md) | opus | 代码审查，重点关注安全性和生产可靠性 |
| [security-auditor](../plugins/comprehensive-review/agents/security-auditor.md) | opus | 漏洞评估和OWASP合规性 |
| [backend-security-coder](../plugins/data-validation-suite/agents/backend-security-coder.md) | opus | 安全后端编码实践、API安全实现 |
| [frontend-security-coder](../plugins/frontend-mobile-security/agents/frontend-security-coder.md) | opus | XSS预防、CSP实现、客户端安全 |
| [mobile-security-coder](../plugins/frontend-mobile-security/agents/mobile-security-coder.md) | opus | 移动安全模式、WebView安全、生物识别认证 |

##### 测试与调试

| Agent | 模型 | 描述 |
|-------|------|------|
| [test-automator](../plugins/codebase-cleanup/agents/test-automator.md) | sonnet | 全面测试套件创建（单元、集成、端到端） |
| [tdd-orchestrator](../plugins/backend-development/agents/tdd-orchestrator.md) | sonnet | 测试驱动开发方法论指导 |
| [debugger](../plugins/error-debugging/agents/debugger.md) | sonnet | 错误解决和测试失败分析 |
| [error-detective](../plugins/error-debugging/agents/error-detective.md) | sonnet | 日志分析和错误模式识别 |

##### 性能与可观测性

| Agent | 模型 | 描述 |
|-------|------|------|
| [performance-engineer](../plugins/observability-monitoring/agents/performance-engineer.md) | opus | 应用程序分析和优化 |
| [observability-engineer](../plugins/observability-monitoring/agents/observability-engineer.md) | opus | 生产监控、分布式跟踪、SLI/SLO管理 |
| [search-specialist](../plugins/content-marketing/agents/search-specialist.md) | haiku | 高级网络研究和信息综合 |

#### 数据与AI

##### 数据工程与分析

| Agent | 模型 | 描述 |
|-------|------|------|
| [data-scientist](../plugins/machine-learning-ops/agents/data-scientist.md) | opus | 数据分析、SQL查询、BigQuery操作 |
| [data-engineer](../plugins/data-engineering/agents/data-engineer.md) | sonnet | ETL管道、数据仓库、流式架构 |

##### 机器学习与AI

| Agent | 模型 | 描述 |
|-------|------|------|
| [ai-engineer](../plugins/llm-application-dev/agents/ai-engineer.md) | opus | LLM应用程序、RAG系统、提示管道 |
| [ml-engineer](../plugins/machine-learning-ops/agents/ml-engineer.md) | opus | ML管道、模型服务、特征工程 |
| [mlops-engineer](../plugins/machine-learning-ops/agents/mlops-engineer.md) | opus | ML基础设施、实验跟踪、模型注册表 |
| [prompt-engineer](../plugins/llm-application-dev/agents/prompt-engineer.md) | opus | LLM提示优化和工程 |

#### 文档与技术写作

| Agent | 模型 | 描述 |
|-------|------|------|
| [docs-architect](../plugins/code-documentation/agents/docs-architect.md) | opus | 全面的技术文档生成 |
| [api-documenter](../plugins/api-testing-observability/agents/api-documenter.md) | sonnet | OpenAPI/Swagger规范和开发者文档 |
| [reference-builder](../plugins/documentation-generation/agents/reference-builder.md) | haiku | 技术参考和API文档 |
| [tutorial-engineer](../plugins/code-documentation/agents/tutorial-engineer.md) | sonnet | 分步教程和教育内容 |
| [mermaid-expert](../plugins/documentation-generation/agents/mermaid-expert.md) | sonnet | 图表创建（流程图、序列图、ERD） |

#### 业务与运营

##### 业务分析与金融

| Agent | 模型 | 描述 |
|-------|------|------|
| [business-analyst](../plugins/business-analytics/agents/business-analyst.md) | sonnet | 指标分析、报告、KPI跟踪 |
| [quant-analyst](../plugins/quantitative-trading/agents/quant-analyst.md) | opus | 金融建模、交易策略、市场分析 |
| [risk-manager](../plugins/quantitative-trading/agents/risk-manager.md) | sonnet | 投资组合风险监控和管理 |

##### 营销与销售

| Agent | 模型 | 描述 |
|-------|------|------|
| [content-marketer](../plugins/content-marketing/agents/content-marketer.md) | sonnet | 博客文章、社交媒体、电子邮件营销 |
| [sales-automator](../plugins/customer-sales-automation/agents/sales-automator.md) | haiku | 冷邮件、跟进、提案生成 |

##### 支持与法律

| Agent | 模型 | 描述 |
|-------|------|------|
| [customer-support](../plugins/customer-sales-automation/agents/customer-support.md) | sonnet | 支持工单、FAQ响应、客户沟通 |
| [hr-pro](../plugins/hr-legal-compliance/agents/hr-pro.md) | opus | 人力资源运营、政策、员工关系 |
| [legal-advisor](../plugins/hr-legal-compliance/agents/legal-advisor.md) | opus | 隐私政策、服务条款、法律文档 |

#### SEO与内容优化

| Agent | 模型 | 描述 |
|-------|------|------|
| [seo-content-auditor](../plugins/seo-content-creation/agents/seo-content-auditor.md) | sonnet | 内容质量分析、E-E-A-T信号评估 |
| [seo-meta-optimizer](../plugins/seo-technical-optimization/agents/seo-meta-optimizer.md) | haiku | 元标题和描述优化 |
| [seo-keyword-strategist](../plugins/seo-technical-optimization/agents/seo-keyword-strategist.md) | haiku | 关键词分析和语义变体 |
| [seo-structure-architect](../plugins/seo-technical-optimization/agents/seo-structure-architect.md) | haiku | 内容结构和模式标记 |
| [seo-snippet-hunter](../plugins/seo-technical-optimization/agents/seo-snippet-hunter.md) | haiku | 精选摘要格式化 |
| [seo-content-refresher](../plugins/seo-analysis-monitoring/agents/seo-content-refresher.md) | haiku | 内容新鲜度分析 |
| [seo-cannibalization-detector](../plugins/seo-analysis-monitoring/agents/seo-cannibalization-detector.md) | haiku | 关键词重叠检测 |
| [seo-authority-builder](../plugins/seo-analysis-monitoring/agents/seo-authority-builder.md) | sonnet | E-E-A-T信号分析 |
| [seo-content-writer](../plugins/seo-content-creation/agents/seo-content-writer.md) | sonnet | SEO优化内容创建 |
| [seo-content-planner](../plugins/seo-content-creation/agents/seo-content-planner.md) | haiku | 内容规划和主题集群 |

#### 专业化领域

| Agent | 模型 | 描述 |
|-------|------|------|
| [arm-cortex-expert](../plugins/arm-cortex-microcontrollers/agents/arm-cortex-expert.md) | sonnet | ARM Cortex-M固件和外设驱动开发 |
| [blockchain-developer](../plugins/blockchain-web3/agents/blockchain-developer.md) | sonnet | Web3应用、智能合约、DeFi协议 |
| [payment-integration](../plugins/payment-processing/agents/payment-integration.md) | sonnet | 支付处理器集成（Stripe、PayPal） |
| [legacy-modernizer](../plugins/framework-migration/agents/legacy-modernizer.md) | sonnet | 遗留代码重构和现代化 |
| [context-manager](../plugins/agent-orchestration/agents/context-manager.md) | haiku | 多代理上下文管理 |

### 模型配置

Agents根据任务复杂性和计算需求分配给特定的Claude模型。

#### 模型分布摘要

| 模型 | Agent数量 | 使用场景 |
|------|----------|----------|
| Haiku | 47 | 快速执行任务：测试、文档、运维、数据库优化、业务 |
| Sonnet | 97 | 复杂推理、架构、语言专业知识、编排、安全 |

#### 模型选择标准

##### Haiku - 快速执行与确定性任务

**使用时机：**
- 根据明确定义的规范生成代码
- 遵循既定模式创建测试
- 使用清晰模板编写文档
- 执行基础设施操作
- 执行数据库查询优化
- 处理客户支持响应
- 处理SEO优化任务
- 管理部署管道

##### Sonnet - 复杂推理与架构

**使用时机：**
- 设计系统架构
- 做出技术选择决策
- 执行安全审计
- 审查代码的架构模式
- 创建复杂的AI/ML管道
- 提供语言特定的专业知识
- 编排多代理工作流
- 处理关键业务的法律/人力资源事务

### 混合编排模式

插件生态系统利用Sonnet + Haiku编排，以实现最佳性能和成本效率：

#### 模式1：规划→执行
```
Sonnet: backend-architect (设计API架构)
  ↓
Haiku: 生成遵循规范的API端点
  ↓
Haiku: test-automator (生成全面测试)
  ↓
Sonnet: code-reviewer (架构审查)
```

#### 模式2：推理→行动（事件响应）
```
Sonnet: incident-responder (诊断问题，创建策略)
  ↓
Haiku: devops-troubleshooter (执行修复)
  ↓
Haiku: deployment-engineer (部署热修复)
  ↓
Haiku: 实现监控告警
```

#### 模式3：复杂→简单（数据库设计）
```
Sonnet: database-architect (模式设计，技术选择)
  ↓
Haiku: sql-pro (生成迁移脚本)
  ↓
Haiku: database-admin (执行迁移)
  ↓
Haiku: database-optimizer (调整查询性能)
```

#### 模式4：多代理工作流
```
全栈功能开发：
Sonnet: backend-architect + frontend-developer (设计组件)
  ↓
Haiku: 生成遵循设计的代码
  ↓
Haiku: test-automator (单元+集成测试)
  ↓
Sonnet: security-auditor (安全审查)
  ↓
Haiku: deployment-engineer (CI/CD设置)
  ↓
Haiku: 设置可观测性堆栈
```

### Agent调用

#### 自然语言

可以通过自然语言调用Agents，当您需要Claude推理使用哪个专家时：

```
"使用backend-architect设计认证API"
"让security-auditor扫描OWASP漏洞"
"让performance-engineer优化这个数据库查询"
```

#### 斜杠命令

许多agents可通过插件斜杠命令直接调用：

```bash
/backend-development:feature-development 用户认证
/security-scanning:security-sast
/incident-response:smart-fix "支付服务中的内存泄漏"
```

### 贡献

要添加新agent：

1. 创建`plugins/{plugin-name}/agents/{agent-name}.md`
2. 添加包含名称、描述和模型分配的frontmatter
3. 编写全面的系统提示
4. 更新`.claude-plugin/marketplace.json`中的插件定义

详情请参见[贡献指南](../CONTRIBUTING.md)。

---

## Architecture & Design Principles

此市场遵循行业最佳实践，注重粒度、可组合性和最小化token使用。

### 核心理念

#### 单一职责原则

- 每个插件**做好一件事**（Unix哲学）
- 明确、专注的目的（可用5-10个词描述）
- 平均插件大小：**3.4个组件**（遵循Anthropic的2-8模式）
- **无臃肿插件** - 所有插件都专注且有目的性

#### 可组合性优于捆绑

- 根据需要混合和匹配插件
- 工作流编排器组合专注插件
- 无强制功能捆绑
- 插件间边界清晰

#### 上下文效率

- 更小的工具 = 更快的处理速度
- 更好地适应LLM上下文窗口
- 更准确、专注的响应
- 仅安装所需内容

#### 可维护性

- 单一目的 = 更易更新
- 边界清晰 = 隔离变更
- 减少重复 = 更简单的维护
- 依赖隔离

### 粒度插件架构

#### 插件分布

- **63个专注插件**，针对特定用例优化
- **23个清晰类别**，每个类别1-6个插件，便于发现
- 按领域组织：
  - **开发**：4个插件（调试、后端、前端、多平台）
  - **安全**：4个插件（扫描、合规、后端API、前端移动）
  - **运维**：4个插件（事件、诊断、分布式、可观测性）
  - **语言**：7个插件（Python、JS/TS、系统、JVM、脚本、函数式、嵌入式）
  - **基础设施**：5个插件（部署、验证、K8s、云、CI/CD）
  - 以及18个更多专业类别

#### 组件分解

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

### 仓库结构

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

### 插件结构

每个插件包含：

- **agents/** - 该领域的专业agents（可选）
- **commands/** - 该插件特定的工具和工作流（可选）
- **skills/** - 渐进式披露的模块化知识包（可选）

#### 最低要求

- 至少一个agent或一个command
- 明确、专注的目的
- 所有文件中的适当frontmatter
- marketplace.json中的条目

#### 示例插件

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

### Agent Skills架构

#### 渐进式披露

Skills使用三层架构实现token效率：

1. **元数据** (Frontmatter)：名称和激活条件（始终加载）
2. **指令**：核心指导和模式（激活时加载）
3. **资源**：示例和模板（按需加载）

#### 规范合规性

所有skills都遵循[Agent Skills规范](https://github.com/anthropics/skills/blob/main/agent_skills_spec.md)：

```yaml
---
name: skill-name                  # 必需：连字符命名
description: skill的作用。使用时机[触发器]。 # 必需：< 1024字符
---

# 带有渐进式披露的skill内容
```

#### 优势

- **Token效率**：仅在需要时加载相关知识
- **专业化的专业知识**：深度领域知识而无冗余
- **清晰激活**：明确的触发器防止不必要的调用
- **可组合性**：跨工作流混合和匹配skills
- **可维护性**：隔离更新不影响其他skills

详情请参见[Agent Skills](./agent-skills.md)了解47个skills的完整详情。

### 模型配置策略

#### 双层架构

系统战略性地使用Claude Opus和Sonnet模型：

| 模型 | 数量 | 使用场景 |
|------|------|----------|
| Haiku | 47个agents | 快速执行、确定性任务 |
| Sonnet | 97个agents | 复杂推理、架构决策 |

#### 选择标准

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

#### 混合编排

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

### 性能与质量

#### 优化的Token使用

- **隔离插件** 仅加载所需内容
- **粒度架构** 减少不必要的上下文
- **渐进式披露** (skills) 按需加载知识
- **清晰边界** 防止上下文污染

#### 组件覆盖

- **100% agent覆盖** - 所有插件至少包含一个agent
- **100% 组件可用性** - 所有85个agents在插件中可访问
- **高效分布** - 平均每个插件3.4个组件

#### 可发现性

- **清晰的插件名称** 立即传达目的
- **逻辑分类** 23个明确定义的类别
- **可搜索的文档** 带有交叉引用
- **易于找到** 适合工作的正确工具

### 设计模式

#### 模式1：单一目的插件

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

#### 模式2：工作流编排

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

#### 模式3：Agent + Skill集成

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

#### 模式4：多插件组合

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

### 版本控制与更新

#### 市场更新

- `.claude-plugin/marketplace.json`中的市场目录
- 插件的语义版本控制
- 维护向后兼容性
- 为重大变更提供清晰的迁移指南

#### 插件更新

- 单个插件更新不影响其他插件
- Skills可以独立更新
- Agents可以添加/删除而不破坏工作流
- Commands保持稳定的接口

### 贡献指南

#### 添加插件

1. 创建插件目录：`plugins/{plugin-name}/`
2. 添加agents和/或commands
3. 可选添加skills
4. 更新marketplace.json
5. 在适当类别中记录文档

#### 添加Agent

1. 创建`plugins/{plugin-name}/agents/{agent-name}.md`
2. 添加frontmatter（名称、描述、模型）
3. 编写全面的系统提示
4. 更新插件定义

#### 添加Skill

1. 创建`plugins/{plugin-name}/skills/{skill-name}/SKILL.md`
2. 添加YAML frontmatter（名称、带"Use when"的描述）
3. 使用渐进式披露编写skill内容
4. 将其添加到marketplace.json中的插件skills数组

#### 质量标准

- **清晰命名** - 连字符命名，描述性
- **专注范围** - 单一职责
- **完整文档** - 什么、何时、如何
- **测试功能** - 提交前验证
- **规范合规** - 遵循Anthropic指南

### 参见

- [Agent Skills](./agent-skills.md) - 模块化知识包
- [Agent Reference](./agents.md) - 完整agent目录
- [Plugin Reference](./plugins.md) - 所有63个插件
- [Usage Guide](./usage.md) - 命令和工作流

---

## Complete Plugin Reference

浏览所有**63个专注、单一目的的插件**，按类别组织。

### 快速开始 - 必备插件

> 💡 **刚开始？** 安装这些热门插件以立即获得生产力提升。

#### 开发必备

**code-documentation** - 文档和技术写作

```bash
/plugin install code-documentation
```

自动化文档生成、代码解释和教程创建，用于全面的技术文档。

**debugging-toolkit** - 智能调试和开发者体验

```bash
/plugin install debugging-toolkit
```

交互式调试、错误分析和DX优化，用于更快的问题解决。

**git-pr-workflows** - Git自动化和PR增强

```bash
/plugin install git-pr-workflows
```

Git工作流自动化、拉取请求增强和团队入职流程。

#### 全栈开发

**backend-development** - 后端API设计和架构

```bash
/plugin install backend-development
```

RESTful和GraphQL API设计，采用测试驱动开发和现代后端架构模式。

**frontend-mobile-development** - UI和移动开发

```bash
/plugin install frontend-mobile-development
```

React/React Native组件开发，具有自动化脚手架和跨平台实现。

**full-stack-orchestration** - 端到端功能开发

```bash
/plugin install full-stack-orchestration
```

多代理协调从后端→前端→测试→安全→部署。

#### 测试与质量

**unit-testing** - 自动化测试生成

```bash
/plugin install unit-testing
```

自动生成pytest (Python) 和Jest (JavaScript) 单元测试，具有全面的边缘情况覆盖。

**code-review-ai** - AI驱动的代码审查

```bash
/plugin install code-review-ai
```

架构分析、安全评估和代码质量审查，具有可操作的反馈。

#### 基础设施与运维

**cloud-infrastructure** - 云架构设计

```bash
/plugin install cloud-infrastructure
```

AWS/Azure/GCP架构、Kubernetes设置、Terraform IaC和多云成本优化。

**incident-response** - 生产事件管理

```bash
/plugin install incident-response
```

快速事件分类、根本原因分析和自动化解决工作流，用于生产系统。

#### 语言支持

**python-development** - Python项目脚手架

```bash
/plugin install python-development
```

FastAPI/Django项目初始化，采用现代工具(uv, ruff)和生产就绪架构。

**javascript-typescript** - JavaScript/TypeScript脚手架

```bash
/plugin install javascript-typescript
```

Next.js、React + Vite和Node.js项目设置，采用pnpm和TypeScript最佳实践。

---

### 完整插件目录

#### 🎨 开发 (4个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **debugging-toolkit** | 交互式调试和DX优化 | `/plugin install debugging-toolkit` |
| **backend-development** | 后端API设计，包括GraphQL和TDD | `/plugin install backend-development` |
| **frontend-mobile-development** | 前端UI和移动开发 | `/plugin install frontend-mobile-development` |
| **multi-platform-apps** | 跨平台应用协调(web/iOS/Android) | `/plugin install multi-platform-apps` |

#### 📚 文档 (2个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **code-documentation** | 文档生成和代码解释 | `/plugin install code-documentation` |
| **documentation-generation** | OpenAPI规范、Mermaid图表、教程 | `/plugin install documentation-generation` |

#### 🔄 工作流 (3个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **git-pr-workflows** | Git自动化和PR增强 | `/plugin install git-pr-workflows` |
| **full-stack-orchestration** | 端到端功能编排 | `/plugin install full-stack-orchestration` |
| **tdd-workflows** | 测试驱动开发方法论 | `/plugin install tdd-workflows` |

#### ✅ 测试 (2个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **unit-testing** | 自动化单元测试生成(Python/JavaScript) | `/plugin install unit-testing` |
| **tdd-workflows** | 测试驱动开发方法论 | `/plugin install tdd-workflows` |

#### 🔍 质量 (3个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **code-review-ai** | AI驱动的架构审查 | `/plugin install code-review-ai` |
| **comprehensive-review** | 多视角代码分析 | `/plugin install comprehensive-review` |
| **performance-testing-review** | 性能分析和测试覆盖率审查 | `/plugin install performance-testing-review` |

#### 🛠️ 实用工具 (4个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **code-refactoring** | 代码清理和技术债务管理 | `/plugin install code-refactoring` |
| **dependency-management** | 依赖审计和版本管理 | `/plugin install dependency-management` |
| **error-debugging** | 错误分析和跟踪调试 | `/plugin install error-debugging` |
| **team-collaboration** | 团队工作流和站会自动化 | `/plugin install team-collaboration` |

#### 🤖 AI & ML (4个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **llm-application-dev** | LLM应用和提示工程 | `/plugin install llm-application-dev` |
| **agent-orchestration** | 多代理系统优化 | `/plugin install agent-orchestration` |
| **context-management** | 上下文持久化和恢复 | `/plugin install context-management` |
| **machine-learning-ops** | ML训练管道和MLOps | `/plugin install machine-learning-ops` |

#### 📊 数据 (2个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **data-engineering** | ETL管道和数据仓库 | `/plugin install data-engineering` |
| **data-validation-suite** | 模式验证和数据质量 | `/plugin install data-validation-suite` |

#### 🗄️ 数据库 (2个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **database-design** | 数据库架构和模式设计 | `/plugin install database-design` |
| **database-migrations** | 数据库迁移自动化 | `/plugin install database-migrations` |

#### 🚨 运维 (4个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **incident-response** | 生产事件管理 | `/plugin install incident-response` |
| **error-diagnostics** | 错误跟踪和根本原因分析 | `/plugin install error-diagnostics` |
| **distributed-debugging** | 分布式系统跟踪 | `/plugin install distributed-debugging` |
| **observability-monitoring** | 指标、日志、跟踪和SLO | `/plugin install observability-monitoring` |

#### ⚡ 性能 (2个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **application-performance** | 应用程序分析和优化 | `/plugin install application-performance` |
| **database-cloud-optimization** | 数据库查询和云成本优化 | `/plugin install database-cloud-optimization` |

#### ☁️ 基础设施 (5个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **deployment-strategies** | 部署模式和回滚自动化 | `/plugin install deployment-strategies` |
| **deployment-validation** | 部署前检查和验证 | `/plugin install deployment-validation` |
| **kubernetes-operations** | K8s清单和GitOps工作流 | `/plugin install kubernetes-operations` |
| **cloud-infrastructure** | AWS/Azure/GCP云架构 | `/plugin install cloud-infrastructure` |
| **cicd-automation** | CI/CD管道配置 | `/plugin install cicd-automation` |

#### 🔒 安全 (4个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **security-scanning** | SAST分析和漏洞扫描 | `/plugin install security-scanning` |
| **security-compliance** | SOC2/HIPAA/GDPR合规 | `/plugin install security-compliance` |
| **backend-api-security** | API安全和认证 | `/plugin install backend-api-security` |
| **frontend-mobile-security** | XSS/CSRF预防和移动安全 | `/plugin install frontend-mobile-security` |

#### 🔄 现代化 (2个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **framework-migration** | 框架升级和迁移规划 | `/plugin install framework-migration` |
| **codebase-cleanup** | 技术债务减少和清理 | `/plugin install codebase-cleanup` |

#### 🌐 API (2个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **api-scaffolding** | REST/GraphQL API生成 | `/plugin install api-scaffolding` |
| **api-testing-observability** | API测试和监控 | `/plugin install api-testing-observability` |

#### 📢 营销 (4个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **seo-content-creation** | SEO内容写作和规划 | `/plugin install seo-content-creation` |
| **seo-technical-optimization** | 元标签、关键词和模式标记 | `/plugin install seo-technical-optimization` |
| **seo-analysis-monitoring** | 内容分析和权威性建设 | `/plugin install seo-analysis-monitoring` |
| **content-marketing** | 内容策略和网络研究 | `/plugin install content-marketing` |

#### 💼 业务 (3个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **business-analytics** | KPI跟踪和财务报告 | `/plugin install business-analytics` |
| **hr-legal-compliance** | HR政策和法律模板 | `/plugin install hr-legal-compliance` |
| **customer-sales-automation** | 支持和销售自动化 | `/plugin install customer-sales-automation` |

#### 💻 语言 (7个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **python-development** | Python 3.12+，包括Django/FastAPI | `/plugin install python-development` |
| **javascript-typescript** | JavaScript/TypeScript，包括Node.js | `/plugin install javascript-typescript` |
| **systems-programming** | Rust、Go、C、C++用于系统开发 | `/plugin install systems-programming` |
| **jvm-languages** | Java、Scala、C#与企业模式 | `/plugin install jvm-languages` |
| **web-scripting** | PHP和Ruby用于Web应用 | `/plugin install web-scripting` |
| **functional-programming** | Elixir，包括OTP和Phoenix | `/plugin install functional-programming` |
| **arm-cortex-microcontrollers** | ARM Cortex-M固件和驱动 | `/plugin install arm-cortex-microcontrollers` |

#### 🔗 区块链 (1个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **blockchain-web3** | 智能合约和DeFi协议 | `/plugin install blockchain-web3` |

#### 💰 金融 (1个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **quantitative-trading** | 算法交易和风险管理 | `/plugin install quantitative-trading` |

#### 💳 支付 (1个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **payment-processing** | Stripe/PayPal集成和计费 | `/plugin install payment-processing` |

#### 🎮 游戏 (1个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **game-development** | Unity和Minecraft插件开发 | `/plugin install game-development` |

#### ♿ 无障碍 (1个插件)

| 插件 | 描述 | 安装 |
|------|------|------|
| **accessibility-compliance** | WCAG审计和包容性设计 | `/plugin install accessibility-compliance` |

### 插件结构

每个插件包含：

- **agents/** - 该领域的专业agents
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

### 安装

#### 步骤1：添加市场

```bash
/plugin marketplace add wshobson/agents
```

这使得所有63个插件都可用于安装，但**不会将任何agents或工具加载到您的上下文中**。

#### 步骤2：安装特定插件

浏览可用插件：

```bash
/plugin
```

仅安装您需要的插件：

```bash
/plugin install python-development
/plugin install backend-development
```

每个已安装的插件仅将其特定的agents和commands加载到Claude的上下文中。

### 插件设计原则

#### 单一职责
- 每个插件**做好一件事**（Unix哲学）
- 明确、专注的目的（可用5-10个词描述）
- 平均插件大小：**3.4个组件**（遵循Anthropic的2-8模式）

#### 最小化Token使用
- 仅安装所需内容
- 每个插件仅加载其特定的agents和工具
- 无不必要的资源加载到上下文中
- 通过粒度插件实现更好的上下文效率

#### 可组合性
- 混合和匹配插件以实现复杂工作流
- 工作流编排器组合专注插件
- 插件间边界清晰
- 无强制功能捆绑

### 参见

- [Agent Skills](./agent-skills.md) - 跨插件的47个专业skills
- [Agent Reference](./agents.md) - 完整agent目录
- [Usage Guide](./usage.md) - 命令和工作流
- [Architecture](./architecture.md) - 设计原则

---

## Usage Guide

使用agents、斜杠命令和多代理工作流的完整指南。

### 概述

插件生态系统提供两个主要接口：

1. **斜杠命令** - 直接调用工具和工作流
2. **自然语言** - Claude推理使用哪个agents

### 斜杠命令

斜杠命令是使用agents和工作流的主要接口。每个插件提供命名空间命令，您可以直接运行。

#### 命令格式

```bash
/plugin-name:command-name [参数]
```

#### 发现命令

列出所有来自已安装插件的斜杠命令：

```bash
/plugin
```

#### 斜杠命令的优势

- **直接调用** - 无需用自然语言描述您想要什么
- **结构化参数** - 显式传递参数以实现精确控制
- **可组合性** - 链接命令以实现复杂工作流
- **可发现性** - 使用`/plugin`查看所有可用命令

### 自然语言

Agents也可以通过自然语言调用，当您需要Claude推理使用哪个专家时：

```
"使用backend-architect设计认证API"
"让security-auditor扫描OWASP漏洞"
"让performance-engineer优化这个数据库查询"
```

Claude Code根据您的请求自动选择和协调适当的agents。

### 按类别分类的命令参考

#### 开发与功能

| 命令 | 描述 |
|------|------|
| `/backend-development:feature-development` | 端到端后端功能开发 |
| `/full-stack-orchestration:full-stack-feature` | 完整的全栈功能实现 |
| `/multi-platform-apps:multi-platform` | 跨平台应用开发协调 |

#### 测试与质量

| 命令 | 描述 |
|------|------|
| `/unit-testing:test-generate` | 生成全面的单元测试 |
| `/tdd-workflows:tdd-cycle` | 完整的TDD红绿重构周期 |
| `/tdd-workflows:tdd-red` | 首先编写失败测试 |
| `/tdd-workflows:tdd-green` | 实现代码以通过测试 |
| `/tdd-workflows:tdd-refactor` | 重构通过的测试 |

#### 代码质量与审查

| 命令 | 描述 |
|------|------|
| `/code-review-ai:ai-review` | AI驱动的代码审查 |
| `/comprehensive-review:full-review` | 多视角分析 |
| `/comprehensive-review:pr-enhance` | 增强拉取请求 |

#### 调试与故障排除

| 命令 | 描述 |
|------|------|
| `/debugging-toolkit:smart-debug` | 交互式智能调试 |
| `/incident-response:incident-response` | 生产事件管理 |
| `/incident-response:smart-fix` | 自动化事件解决 |
| `/error-debugging:error-analysis` | 深度错误分析 |
| `/error-debugging:error-trace` | 堆栈跟踪调试 |
| `/error-diagnostics:smart-debug` | 智能诊断调试 |
| `/distributed-debugging:debug-trace` | 分布式系统跟踪 |

#### 安全

| 命令 | 描述 |
|------|------|
| `/security-scanning:security-hardening` | 全面的安全加固 |
| `/security-scanning:security-sast` | 静态应用程序安全测试 |
| `/security-scanning:security-dependencies` | 依赖漏洞扫描 |
| `/security-compliance:compliance-check` | SOC2/HIPAA/GDPR合规 |
| `/frontend-mobile-security:xss-scan` | XSS漏洞扫描 |

#### 基础设施与部署

| 命令 | 描述 |
|------|------|
| `/observability-monitoring:monitor-setup` | 设置监控基础设施 |
| `/observability-monitoring:slo-implement` | 实现SLO/SLI指标 |
| `/deployment-validation:config-validate` | 部署前验证 |
| `/cicd-automation:workflow-automate` | CI/CD管道自动化 |

#### 数据与ML

| 命令 | 描述 |
|------|------|
| `/machine-learning-ops:ml-pipeline` | ML训练管道编排 |
| `/data-engineering:data-pipeline` | ETL/ELT管道构建 |
| `/data-engineering:data-driven-feature` | 数据驱动功能开发 |

#### 文档

| 命令 | 描述 |
|------|------|
| `/code-documentation:doc-generate` | 生成全面的文档 |
| `/code-documentation:code-explain` | 解释代码功能 |
| `/documentation-generation:doc-generate` | OpenAPI规范、图表、教程 |

#### 重构与维护

| 命令 | 描述 |
|------|------|
| `/code-refactoring:refactor-clean` | 代码清理和重构 |
| `/code-refactoring:tech-debt` | 技术债务管理 |
| `/codebase-cleanup:deps-audit` | 依赖审计 |
| `/codebase-cleanup:tech-debt` | 技术债务减少 |
| `/framework-migration:legacy-modernize` | 遗留代码现代化 |
| `/framework-migration:code-migrate` | 框架迁移 |
| `/framework-migration:deps-upgrade` | 依赖升级 |

#### 数据库

| 命令 | 描述 |
|------|------|
| `/database-migrations:sql-migrations` | SQL迁移自动化 |
| `/database-migrations:migration-observability` | 迁移监控 |
| `/database-cloud-optimization:cost-optimize` | 数据库和云优化 |

#### Git与PR工作流

| 命令 | 描述 |
|------|------|
| `/git-pr-workflows:pr-enhance` | 增强拉取请求质量 |
| `/git-pr-workflows:onboard` | 团队入职自动化 |
| `/git-pr-workflows:git-workflow` | Git工作流自动化 |

#### 项目脚手架

| 命令 | 描述 |
|------|------|
| `/python-development:python-scaffold` | FastAPI/Django项目设置 |
| `/javascript-typescript:typescript-scaffold` | Next.js/React + Vite设置 |
| `/systems-programming:rust-project` | Rust项目脚手架 |

#### AI与LLM开发

| 命令 | 描述 |
|------|------|
| `/llm-application-dev:langchain-agent` | LangChain agent开发 |
| `/llm-application-dev:ai-assistant` | AI助手实现 |
| `/llm-application-dev:prompt-optimize` | 提示工程优化 |
| `/agent-orchestration:multi-agent-optimize` | 多代理优化 |
| `/agent-orchestration:improve-agent` | Agent改进工作流 |

#### 测试与性能

| 命令 | 描述 |
|------|------|
| `/performance-testing-review:ai-review` | 性能分析 |
| `/application-performance:performance-optimization` | 应用优化 |

#### 团队协作

| 命令 | 描述 |
|------|------|
| `/team-collaboration:issue` | 问题管理自动化 |
| `/team-collaboration:standup-notes` | 站会笔记生成 |

#### 无障碍

| 命令 | 描述 |
|------|------|
| `/accessibility-compliance:accessibility-audit` | WCAG合规审计 |

#### API开发

| 命令 | 描述 |
|------|------|
| `/api-testing-observability:api-mock` | API模拟和测试 |

#### 上下文管理

| 命令 | 描述 |
|------|------|
| `/context-management:context-save` | 保存对话上下文 |
| `/context-management:context-restore` | 恢复先前上下文 |

### 多代理工作流示例

插件提供预配置的多代理工作流，可通过斜杠命令访问。

#### 全栈开发

```bash
# 基于命令的工作流调用
/full-stack-orchestration:full-stack-feature "带实时分析的用户仪表板"

# 自然语言替代
"实现带实时分析的用户仪表板"
```

**编排：** backend-architect → database-architect → frontend-developer → test-automator → security-auditor → deployment-engineer → observability-engineer

**发生什么：**

1. 数据库模式设计与迁移
2. 后端API实现（REST/GraphQL）
3. 前端组件与状态管理
4. 全面测试套件（单元/集成/端到端）
5. 安全审计和加固
6. CI/CD管道设置与功能标志
7. 可观测性和监控配置

#### 安全加固

```bash
# 全面的安全评估和修复
/security-scanning:security-hardening --level comprehensive

# 自然语言替代
"执行安全审计并实施OWASP最佳实践"
```

**编排：** security-auditor → backend-security-coder → frontend-security-coder → mobile-security-coder → test-automator

#### 数据/ML管道

```bash
# ML功能开发与生产部署
/machine-learning-ops:ml-pipeline "客户流失预测模型"

# 自然语言替代
"构建客户流失预测模型与部署"
```

**编排：** data-scientist → data-engineer → ml-engineer → mlops-engineer → performance-engineer

#### 事件响应

```bash
# 智能调试与根本原因分析
/incident-response:smart-fix "支付服务中的生产内存泄漏"

# 自然语言替代
"调试生产内存泄漏并创建运行手册"
```

**编排：** incident-responder → devops-troubleshooter → debugger → error-detective → observability-engineer

### 命令参数和选项

许多斜杠命令支持参数以实现精确控制：

```bash
# 为特定文件生成测试
/unit-testing:test-generate src/api/users.py

# 功能开发与方法论规范
/backend-development:feature-development OAuth2集成与社交登录

# 安全依赖扫描
/security-scanning:security-dependencies

# 组件脚手架
/frontend-mobile-development:component-scaffold UserProfile组件与hooks

# TDD工作流周期
/tdd-workflows:tdd-red 用户可以重置密码
/tdd-workflows:tdd-green
/tdd-workflows:tdd-refactor

# 智能调试
/debugging-toolkit:smart-debug 结账流程中的内存泄漏

# Python项目脚手架
/python-development:python-scaffold fastapi-microservice
```

### 结合自然语言和命令

您可以混合两种方法以实现最佳灵活性：

```
# 从命令开始进行结构化工作流
/full-stack-orchestration:full-stack-feature "支付处理"

# 然后提供自然语言指导
"确保PCI-DSS合规并集成Stripe"
"添加失败事务的重试逻辑"
"设置欺诈检测规则"
```

### 最佳实践

#### 何时使用斜杠命令

- **结构化工作流** - 多步骤过程，有明确阶段
- **重复任务** - 频繁执行的操作
- **精确控制** - 需要特定参数时
- **发现** - 探索可用功能

#### 何时使用自然语言

- **探索性工作** - 不确定使用哪个工具时
- **复杂推理** - Claude需要协调多个agents时
- **情境决策** - 正确方法取决于情况时
- **临时任务** - 不符合命令的一次性操作

#### 工作流组合

为复杂场景组合多个插件：

```bash
# 1. 从功能开发开始
/backend-development:feature-development 支付处理API

# 2. 添加安全加固
/security-scanning:security-hardening

# 3. 生成全面测试
/unit-testing:test-generate

# 4. 审查实现
/code-review-ai:ai-review

# 5. 设置CI/CD
/cicd-automation:workflow-automate

# 6. 添加监控
/observability-monitoring:monitor-setup
```

### Agent Skills集成

Agent Skills与命令协同工作，提供深度专业知识：

```
用户："设置带异步模式的FastAPI项目"
→ 激活：fastapi-templates skill
→ 调用：/python-development:python-scaffold
→ 结果：生产就绪的FastAPI项目，采用最佳实践

用户："实现带Helm的Kubernetes部署"
→ 激活：helm-chart-scaffolding、k8s-manifest-generator skills
→ 指导：kubernetes-architect agent
→ 结果：生产级K8s清单与Helm图表
```

详情请参见[Agent Skills](./agent-skills.md)了解47个专业skills。

### 参见

- [Agent Skills](./agent-skills.md) - 专业化的知识包
- [Agent Reference](./agents.md) - 完整agent目录
- [Plugin Reference](./plugins.md) - 所有63个插件
- [Architecture](./architecture.md) - 设计原则
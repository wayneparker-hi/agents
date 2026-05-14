# Agent Skills

Agent Skills 是模块化包，通过专门的领域知识扩展 Claude 的功能，遵循 Anthropic 的 [Agent Skills 规范](https://github.com/anthropics/skills/blob/main/agent_skills_spec.md)。这个插件生态系统包括 **153 个专业技能**，跨越 40 个插件，实现渐进式披露和高效的 token 使用。

## 概述

Skills 为 Claude 提供特定领域的深度专业知识，而无需预先将所有内容加载到上下文中。每个 skill 包括：

- **YAML Frontmatter**：名称和激活条件
- **渐进式披露**：元数据→指令→资源
- **激活触发器**：明确的"使用时机"子句，用于自动调用

## 按插件分类的 Skills

### Kubernetes Operations（4 个技能）

| Skill | 描述 |
|-------|------|
| **k8s-manifest-generator** | 创建生产就绪的 Kubernetes 清单，包括 Deployments、Services、ConfigMaps 和 Secrets，遵循最佳实践 |
| **helm-chart-scaffolding** | 设计、组织和管理 Helm 图表，用于模板化和打包 Kubernetes 应用程序 |
| **gitops-workflow** | 使用 ArgoCD 和 Flux 实现 GitOps 工作流，用于自动化、声明式部署 |
| **k8s-security-policies** | 实现 Kubernetes 安全策略，包括 NetworkPolicy、PodSecurityPolicy 和 RBAC |

### LLM Application Development（8 个技能）

| Skill | 描述 |
|-------|------|
| **langchain-architecture** | 使用 LangChain 框架设计 LLM 应用程序，包括 agents、内存和工具集成 |
| **prompt-engineering-patterns** | 掌握高级提示工程技术，以提高 LLM 的性能和可靠性 |
| **rag-implementation** | 构建检索增强生成系统，使用向量数据库和语义搜索 |
| **llm-evaluation** | 实现全面的评估策略，包括自动化指标和基准测试 |
| **embedding-strategies** | 设计文本、图像和多模态内容的嵌入管道，具有最优分块策略 |
| **similarity-search-patterns** | 使用 ANN 算法和距离度量实现高效的相似性搜索 |
| **vector-index-tuning** | 使用 HNSW、IVF 和混合配置优化向量索引性能 |
| **hybrid-search-implementation** | 结合向量和关键词搜索以提高检索准确性 |

### Backend Development（9 个技能）

| Skill | 描述 |
|-------|------|
| **api-design-principles** | 掌握 REST 和 GraphQL API 设计，实现直观、可扩展和可维护的 API |
| **architecture-patterns** | 实现清洁架构、六边形架构和领域驱动设计 |
| **microservices-patterns** | 设计微服务，包括服务边界、事件驱动通信和弹性 |
| **workflow-orchestration-patterns** | 使用 Temporal 为分布式系统设计持久化工作流、saga 模式和状态管理 |
| **temporal-python-testing** | 使用 pytest、时间跳过和 mocking 策略测试 Temporal 工作流，实现全面覆盖 |
| **event-store-design** | 设计具有优化模式、快照和流分区的事件存储 |
| **cqrs-implementation** | 使用分离的读/写模型和最终一致性模式实现 CQRS |
| **projection-patterns** | 从事件流构建高效的投影，用于读优化视图 |
| **saga-orchestration** | 设计具有补偿逻辑和故障处理的分布式 saga |

### Developer Essentials（11 个技能）

| Skill | 描述 |
|-------|------|
| **git-advanced-workflows** | 掌握高级 Git 工作流，包括 rebase、cherry-pick、bisect、worktrees 和 reflog |
| **sql-optimization-patterns** | 优化 SQL 查询、索引策略和 EXPLAIN 分析，以提高数据库性能 |
| **error-handling-patterns** | 实现健壮的错误处理，包括异常、Result 类型和优雅降级 |
| **code-review-excellence** | 提供有效的代码审查，包括建设性反馈和系统分析 |
| **e2e-testing-patterns** | 使用 Playwright 和 Cypress 构建可靠的端到端测试套件，用于关键用户工作流 |
| **auth-implementation-patterns** | 实现身份验证和授权，包括 JWT、OAuth2、会话和 RBAC |
| **debugging-strategies** | 掌握系统调试技术、分析工具和根本原因分析 |
| **monorepo-management** | 使用 Turborepo、Nx 和 pnpm 工作区管理 monorepo，用于可扩展的多包项目 |
| **nx-workspace-patterns** | 使用计算缓存和受影响命令配置 Nx 工作区 |
| **turborepo-caching** | 使用远程缓存和管道配置优化 Turborepo 构建 |
| **bazel-build-optimization** | 设计具有密封操作和远程执行的 Bazel 构建 |

### Blockchain & Web3（4 个技能）

| Skill | 描述 |
|-------|------|
| **defi-protocol-templates** | 实现 DeFi 协议，包括质押、AMMs、治理和借贷的模板 |
| **nft-standards** | 实现 NFT 标准（ERC-721、ERC-1155），包括元数据和市场集成 |
| **solidity-security** | 掌握智能合约安全，防止漏洞并实现安全模式 |
| **web3-testing** | 使用 Hardhat 和 Foundry 测试智能合约，包括单元测试和主网分叉 |

### CI/CD Automation（4 个技能）

| Skill | 描述 |
|-------|------|
| **deployment-pipeline-design** | 设计多阶段 CI/CD 管道，包括审批门和安全检查 |
| **github-actions-templates** | 创建生产就绪的 GitHub Actions 工作流，用于测试、构建和部署 |
| **gitlab-ci-patterns** | 构建 GitLab CI/CD 管道，包括多阶段工作流和分布式运行器 |
| **secrets-management** | 实现安全的密钥管理，使用 Vault、AWS Secrets Manager 或原生解决方案 |

### Cloud Infrastructure（8 个技能）

| Skill | 描述 |
|-------|------|
| **terraform-module-library** | 构建可重用的 Terraform 模块，用于 AWS、Azure 和 GCP 基础设施 |
| **multi-cloud-architecture** | 设计多云架构，避免供应商锁定 |
| **hybrid-cloud-networking** | 配置本地和云平台之间的安全连接 |
| **cost-optimization** | 通过正确的大小调整、标记和预留实例优化云成本 |
| **istio-traffic-management** | 配置 Istio 流量路由、负载均衡和金丝雀部署 |
| **linkerd-patterns** | 使用自动 mTLS 和流量分割实现 Linkerd 服务网格 |
| **mtls-configuration** | 设计具有证书管理的零信任 mTLS 架构 |
| **service-mesh-observability** | 构建具有分布式跟踪和指标的全面可观测性 |

### Framework Migration（4 个技能）

| Skill | 描述 |
|-------|------|
| **react-modernization** | 升级 React 应用，迁移到 hooks，并采用并发特性 |
| **angular-migration** | 使用混合模式和增量重写从 AngularJS 迁移到 Angular |
| **database-migration** | 执行数据库迁移，采用零停机策略和转换 |
| **dependency-upgrade** | 管理主要依赖升级，包括兼容性分析和测试 |

### Observability & Monitoring（4 个技能）

| Skill | 描述 |
|-------|------|
| **prometheus-configuration** | 设置 Prometheus，用于全面的指标收集和监控 |
| **grafana-dashboards** | 创建生产 Grafana 仪表板，用于实时系统可视化 |
| **distributed-tracing** | 实现分布式跟踪，使用 Jaeger 和 Tempo 跟踪请求 |
| **slo-implementation** | 定义 SLIs 和 SLOs，包括错误预算和告警 |

### Payment Processing（4 个技能）

| Skill | 描述 |
|-------|------|
| **stripe-integration** | 实现 Stripe 支付处理，包括结账、订阅和 webhooks |
| **paypal-integration** | 集成 PayPal 支付处理，包括快速结账和订阅 |
| **pci-compliance** | 实现 PCI DSS 合规，用于安全支付卡数据处理 |
| **billing-automation** | 构建自动化计费系统，用于经常性支付和发票 |

### Python Development（16 个技能）

| Skill | 描述 |
|-------|------|
| **async-python-patterns** | 掌握 Python asyncio、并发编程和 async/await 模式 |
| **python-testing-patterns** | 实现全面的测试，包括 pytest、fixtures 和 mocking |
| **python-packaging** | 创建可分发的 Python 包，包括适当的结构和 PyPI 发布 |
| **python-performance-optimization** | 使用 cProfile 和性能最佳实践分析和优化 Python 代码 |
| **uv-package-manager** | 掌握 uv 包管理器，用于快速依赖管理和虚拟环境 |

### JavaScript/TypeScript（4 个技能）

| Skill | 描述 |
|-------|------|
| **typescript-advanced-types** | 掌握 TypeScript 的高级类型系统，包括泛型和条件类型 |
| **nodejs-backend-patterns** | 构建生产就绪的 Node.js 服务，包括 Express/Fastify 和最佳实践 |
| **javascript-testing-patterns** | 实现全面的测试，包括 Jest、Vitest 和 Testing Library |
| **modern-javascript-patterns** | 掌握 ES6+ 特性，包括 async/await、解构和函数式编程 |

### API Scaffolding（1 个技能）

| Skill | 描述 |
|-------|------|
| **fastapi-templates** | 创建生产就绪的 FastAPI 项目，包括异步模式和错误处理 |

### Machine Learning Operations（1 个技能）

| Skill | 描述 |
|-------|------|
| **ml-pipeline-workflow** | 构建端到端的 MLOps 管道，从数据准备到部署 |

### Security Scanning（5 个技能）

| Skill | 描述 |
|-------|------|
| **sast-configuration** | 配置静态应用程序安全测试工具，用于漏洞检测 |
| **stride-analysis-patterns** | 应用 STRIDE 方法论识别欺骗、篡改和其他威胁 |
| **attack-tree-construction** | 构建攻击树，将威胁场景映射到漏洞 |
| **security-requirement-extraction** | 从威胁模型提取具有验收标准的安全需求 |
| **threat-mitigation-mapping** | 将威胁映射到缓解措施，制定优先修复计划 |

### Accessibility Compliance（2 个技能）

| Skill | 描述 |
|-------|------|
| **wcag-audit-patterns** | 使用自动化和手动测试进行 WCAG 2.2 无障碍审计 |
| **screen-reader-testing** | 测试跨 NVDA、JAWS 和 VoiceOver 的屏幕阅读器兼容性 |

### Business Analytics（2 个技能）

| Skill | 描述 |
|-------|------|
| **kpi-dashboard-design** | 设计具有可操作 KPI 和下钻功能的执行仪表板 |
| **data-storytelling** | 将数据洞察转化为对利益相关者有说服力的叙述 |

### Data Engineering（4 个技能）

| Skill | 描述 |
|-------|------|
| **spark-optimization** | 使用分区、缓存和广播 join 优化 Apache Spark 作业 |
| **dbt-transformation-patterns** | 使用增量策略和测试构建 dbt 模型 |
| **airflow-dag-patterns** | 设计具有适当依赖关系和错误处理的 Airflow DAG |
| **data-quality-frameworks** | 使用 Great Expectations 和自定义验证器实现数据质量检查 |

### Documentation Generation（3 个技能）

| Skill | 描述 |
|-------|------|
| **openapi-spec-generation** | 从代码生成具有完整模式的 OpenAPI 3.1 规范 |
| **changelog-automation** | 从常规提交自动生成变更日志 |
| **architecture-decision-records** | 编写记录架构决策和权衡的 ADR |

### Frontend Mobile Development（4 个技能）

| Skill | 描述 |
|-------|------|
| **react-state-management** | 使用 Zustand、Jotai 和 React Query 实现状态管理 |
| **nextjs-app-router-patterns** | 使用 App Router、RSC 和流式传输构建 Next.js 14+ 应用 |
| **tailwind-design-system** | 使用 Tailwind CSS 和组件库创建设计系统 |
| **react-native-architecture** | 使用导航和原生模块架构 React Native 应用 |

### UI Design（9 个技能）

| Skill | 描述 |
|-------|------|
| **design-system-patterns** | 构建具有令牌、组件和主题的可扩展设计系统 |
| **accessibility-compliance** | 使用适当的 ARIA 和键盘导航实现 WCAG 2.1/2.2 合规 |
| **responsive-design** | 使用 CSS Grid、Flexbox 和容器查询创建流体布局 |
| **mobile-ios-design** | 按照 Human Interface Guidelines 设计 iOS 应用 |
| **mobile-android-design** | 按照 Material Design 3 指南设计 Android 应用 |
| **react-native-design** | React Native 应用的跨平台设计模式 |
| **web-component-design** | 使用 Shadow DOM 构建可访问、可重用的 Web 组件 |
| **interaction-design** | 创建微交互、动画和基于手势的界面 |
| **visual-design-foundations** | 应用排版、色彩理论、间距和视觉层次 |

### Game Development（2 个技能）

| Skill | 描述 |
|-------|------|
| **unity-ecs-patterns** | 为高性能游戏系统实现 Unity ECS |
| **godot-gdscript-patterns** | 使用 GDScript 最佳实践和场景组合构建 Godot 游戏 |

### HR Legal Compliance（2 个技能）

| Skill | 描述 |
|-------|------|
| **gdpr-data-handling** | 实现具有同意管理的 GDPR 合规数据处理 |
| **employment-contract-templates** | 生成具有特定司法管辖区条款的就业合同 |

### Incident Response（3 个技能）

| Skill | 描述 |
|-------|------|
| **postmortem-writing** | 编写具有根本原因分析和行动项目的无责任事后分析 |
| **incident-runbook-templates** | 为常见事件场景创建包含升级路径的运行手册 |
| **on-call-handoff-patterns** | 设计具有上下文保留和告警路由的值班交接 |

### Quantitative Trading（2 个技能）

| Skill | 描述 |
|-------|------|
| **backtesting-frameworks** | 构建具有实际滑点和交易成本的回测系统 |
| **risk-metrics-calculation** | 计算投资组合的 VaR、夏普比率和最大回撤指标 |

### Systems Programming（3 个技能）

| Skill | 描述 |
|-------|------|
| **rust-async-patterns** | 使用 Tokio、futures 和适当的错误处理实现异步 Rust |
| **go-concurrency-patterns** | 使用 channels、worker pools 和 context cancellation 设计 Go 并发 |
| **memory-safety-patterns** | 使用所有权、边界检查和 sanitizers 编写内存安全代码 |

### Conductor - Project Management（3 个技能）

| Skill | 描述 |
|-------|------|
| **context-driven-development** | 应用上下文驱动开发方法论，包括产品上下文、规范和分阶段规划 |
| **track-management** | 管理功能、错误、杂务和重构的开发轨道，包括规范和实现计划 |
| **workflow-patterns** | 实现 TDD 工作流、提交策略和验证检查点，用于系统化开发 |

### Agent Teams（6 个技能）

| Skill | 描述 |
|-------|------|
| **multi-reviewer-patterns** | 跨质量维度协调并行代码审查，包括去重和严重性校准 |
| **parallel-debugging** | 使用竞争假设、并行调查和根本原因仲裁调试复杂问题 |
| **parallel-feature-development** | 协调并行功能工作，包括文件所有权、冲突避免和集成模式 |
| **task-coordination-strategies** | 分解复杂任务、设计依赖关系图并在多代理团队中平衡工作负载 |
| **team-communication-protocols** | 代理团队的结构化消息传递：消息类型、计划审批和关闭程序 |
| **team-composition-patterns** | 设计最优团队组成，包括规模启发式、预设和代理类型选择 |

### Reverse Engineering（4 个技能）

| Skill | 描述 |
|-------|------|
| **anti-reversing-techniques** | 了解分析过程中遇到的反逆向、混淆和保护技术 |
| **binary-analysis-patterns** | 反汇编、反编译、控制流分析和代码模式识别 |
| **memory-forensics** | 使用 Volatility 和相关工具进行内存获取、进程分析和工件提取 |
| **protocol-reverse-engineering** | 网络协议逆向工程，包括数据包分析和自定义协议文档 |

### Startup Business Analyst（5 个技能）

| Skill | 描述 |
|-------|------|
| **competitive-landscape** | 使用波特五力模型和相关模型进行竞争分析、差异化和定位 |
| **market-sizing-analysis** | 使用自上而下、自下而上和价值理论方法计算 TAM/SAM/SOM |
| **startup-financial-modeling** | 包含收入、成本、现金流和情景规划的 3-5 年财务模型 |
| **startup-metrics-framework** | 从种子轮到 A 轮跟踪和优化关键 SaaS、市场、消费者和 B2B 初创指标 |
| **team-composition-analysis** | 早期初创公司的招聘计划、组织结构、薪酬和股权分配 |

### Shell Scripting（3 个技能）

| Skill | 描述 |
|-------|------|
| **bash-defensive-patterns** | 用于生产级脚本的防御性 Bash 编程技术 |
| **bats-testing-patterns** | 用于全面 shell 脚本测试的 Bash 自动化测试系统（Bats） |
| **shellcheck-configuration** | 用于 shell 脚本质量的 ShellCheck 静态分析配置和使用 |

### Database Design（1 个技能）

| Skill | 描述 |
|-------|------|
| **postgresql-table-design** | 使用适当建模设计和审查 PostgreSQL 特定模式 |

### Documentation Standards（1 个技能）

| Skill | 描述 |
|-------|------|
| **hads** | HADS（人机文档标准）— 用于 token 高效 AI 阅读的语义 Markdown 标记 |

### .NET Contribution（1 个技能）

| Skill | 描述 |
|-------|------|
| **dotnet-backend-patterns** | 用于健壮 API、MCP 服务器和企业应用的 C#/.NET 后端模式 |

### Plugin Eval（1 个技能）

| Skill | 描述 |
|-------|------|
| **evaluation-methodology** | PluginEval 质量方法论 — 维度、评分标准、统计方法、评分公式 |

### Block No-Verify（1 个技能）

| Skill | 描述 |
|-------|------|
| **block-no-verify-hook** | PreToolUse hook，防止 AI 代理通过绕过标志跳过 git pre-commit hooks |

### Protect MCP（1 个技能）

| Skill | 描述 |
|-------|------|
| **protect-mcp-setup** | 为工具调用配置 Cedar 策略执行和 Ed25519 签名收据；包含研究/开发/生产的示例策略 |

## Skills 如何工作

### 激活

当 Claude 在您的请求中检测到匹配模式时，Skills 会自动激活：

```
用户："设置 Kubernetes 部署与 Helm 图表"
→ 激活：helm-chart-scaffolding、k8s-manifest-generator

用户："构建用于文档问答的 RAG 系统"
→ 激活：rag-implementation、prompt-engineering-patterns

用户："优化 Python 异步性能"
→ 激活：async-python-patterns、python-performance-optimization
```

### 渐进式披露

Skills 使用三层架构实现 token 效率：

1. **元数据** (Frontmatter)：名称和激活条件（始终加载）
2. **指令**：核心指导和模式（激活时加载）
3. **资源**：示例和模板（按需加载）

### 与 Agents 的集成

Skills 与 agents 协同工作，提供深度领域专业知识：

- **Agents**：高级推理和编排
- **Skills**：专业化的知识和实现模式

示例工作流：

```
backend-architect agent → 计划 API 架构
  ↓
api-design-principles skill → 提供 REST/GraphQL 最佳实践
  ↓
fastapi-templates skill → 提供生产就绪模板
```

## 规范合规性

所有 153 个 skills 都遵循 [Agent Skills 规范](https://agentskills.io/specification)：

- ✓ 必需的 `name` 字段（连字符命名）
- ✓ 必需的 `description` 字段，带有"Use when"子句
- ✓ 描述少于 1024 个字符
- ✓ 完整、非截断的描述
- ✓ 正确的 YAML frontmatter 格式

## 创建新 Skills

要向插件添加 skill：

1. 创建 `plugins/{plugin-name}/skills/{skill-name}/SKILL.md`
2. 添加 YAML frontmatter：
   ```yaml
   ---
   name: skill-name
   description: skill 的作用。使用时机 [激活触发器]。
   ---
   ```
3. 使用渐进式披露编写全面的 skill 内容
4. 将 skill 路径添加到 `marketplace.json`：
   ```json
   {
     "name": "plugin-name",
     "skills": ["./skills/skill-name"]
   }
   ```

### Skill 结构

```
plugins/{plugin-name}/
└── skills/
    └── {skill-name}/
        └── SKILL.md        # Frontmatter + 内容
```

## 优势

- **Token 效率**：仅在需要时加载相关知识
- **专业化的专业知识**：深度领域知识而无冗余
- **清晰激活**：明确的触发器防止不必要的调用
- **可组合性**：跨工作流混合和匹配 skills
- **可维护性**：隔离更新不影响其他 skills

## 资源

- [Anthropic Skills 仓库](https://github.com/anthropics/skills)
- [Agent Skills 文档](https://docs.claude.com/en/docs/claude-code/skills)

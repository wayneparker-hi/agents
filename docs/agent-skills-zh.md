# Agent Skills

Agent Skills是模块化包，通过专门的领域知识扩展Claude的功能，遵循Anthropic的[Agent Skills规范](https://github.com/anthropics/skills/blob/main/agent_skills_spec.md)。这个插件生态系统包括**55个专业技能**，跨越15个插件，实现渐进式披露和高效的token使用。

## 概述

Skills为Claude提供特定领域的深度专业知识，而无需预先将所有内容加载到上下文中。每个skill包括：

- **YAML Frontmatter**：名称和激活条件
- **渐进式披露**：元数据→指令→资源
- **激活触发器**：明确的"使用时机"子句，用于自动调用

## 按插件分类的Skills

### Kubernetes Operations (4个技能)

| Skill | 描述 |
|-------|------|
| **k8s-manifest-generator** | 创建生产就绪的Kubernetes清单，包括Deployments、Services、ConfigMaps和Secrets，遵循最佳实践 |
| **helm-chart-scaffolding** | 设计、组织和管理Helm图表，用于模板化和打包Kubernetes应用程序 |
| **gitops-workflow** | 使用ArgoCD和Flux实现GitOps工作流，用于自动化、声明式部署 |
| **k8s-security-policies** | 实现Kubernetes安全策略，包括NetworkPolicy、PodSecurityPolicy和RBAC |

### LLM Application Development (4个技能)

| Skill | 描述 |
|-------|------|
| **langchain-architecture** | 使用LangChain框架设计LLM应用程序，包括agents、内存和工具集成 |
| **prompt-engineering-patterns** | 掌握高级提示工程技术，以提高LLM的性能和可靠性 |
| **rag-implementation** | 构建检索增强生成系统，使用向量数据库和语义搜索 |
| **llm-evaluation** | 实现全面的评估策略，包括自动化指标和基准测试 |

### Backend Development (3个技能)

| Skill | 描述 |
|-------|------|
| **api-design-principles** | 掌握REST和GraphQL API设计，实现直观、可扩展和可维护的API |
| **architecture-patterns** | 实现清洁架构、六边形架构和领域驱动设计 |
| **microservices-patterns** | 设计微服务，包括服务边界、事件驱动通信和弹性 |

### Developer Essentials (8个技能)

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

### Blockchain & Web3 (4个技能)

| Skill | 描述 |
|-------|------|
| **defi-protocol-templates** | 实现DeFi协议，包括质押、AMMs、治理和借贷的模板 |
| **nft-standards** | 实现NFT标准(ERC-721、ERC-1155)，包括元数据和市场集成 |
| **solidity-security** | 掌握智能合约安全，防止漏洞并实现安全模式 |
| **web3-testing** | 使用Hardhat和Foundry测试智能合约，包括单元测试和主网分叉 |

### CI/CD Automation (4个技能)

| Skill | 描述 |
|-------|------|
| **deployment-pipeline-design** | 设计多阶段CI/CD管道，包括审批门和安全检查 |
| **github-actions-templates** | 创建生产就绪的GitHub Actions工作流，用于测试、构建和部署 |
| **gitlab-ci-patterns** | 构建GitLab CI/CD管道，包括多阶段工作流和分布式运行器 |
| **secrets-management** | 实现安全的秘密管理，使用Vault、AWS Secrets Manager或原生解决方案 |

### Cloud Infrastructure (4个技能)

| Skill | 描述 |
|-------|------|
| **terraform-module-library** | 构建可重用的Terraform模块，用于AWS、Azure和GCP基础设施 |
| **multi-cloud-architecture** | 设计多云架构，避免供应商锁定 |
| **hybrid-cloud-networking** | 配置本地和云平台之间的安全连接 |
| **cost-optimization** | 通过正确的大小调整、标记和预留实例优化云成本 |

### Framework Migration (4个技能)

| Skill | 描述 |
|-------|------|
| **react-modernization** | 升级React应用，迁移到hooks，并采用并发特性 |
| **angular-migration** | 使用混合模式和增量重写从AngularJS迁移到Angular |
| **database-migration** | 执行数据库迁移，采用零停机策略和转换 |
| **dependency-upgrade** | 管理主要依赖升级，包括兼容性分析和测试 |

### Observability & Monitoring (4个技能)

| Skill | 描述 |
|-------|------|
| **prometheus-configuration** | 设置Prometheus，用于全面的指标收集和监控 |
| **grafana-dashboards** | 创建生产Grafana仪表板，用于实时系统可视化 |
| **distributed-tracing** | 实现分布式跟踪，使用Jaeger和Tempo跟踪请求 |
| **slo-implementation** | 定义SLIs和SLOs，包括错误预算和告警 |

### Payment Processing (4个技能)

| Skill | 描述 |
|-------|------|
| **stripe-integration** | 实现Stripe支付处理，包括结账、订阅和webhooks |
| **paypal-integration** | 集成PayPal支付处理，包括快速结账和订阅 |
| **pci-compliance** | 实现PCI DSS合规，用于安全支付卡数据处理 |
| **billing-automation** | 构建自动化计费系统，用于经常性支付和发票 |

### Python Development (5个技能)

| Skill | 描述 |
|-------|------|
| **async-python-patterns** | 掌握Python asyncio、并发编程和async/await模式 |
| **python-testing-patterns** | 实现全面的测试，包括pytest、fixtures和mocking |
| **python-packaging** | 创建可分发的Python包，包括适当的结构和PyPI发布 |
| **python-performance-optimization** | 使用cProfile和性能最佳实践分析和优化Python代码 |
| **uv-package-manager** | 掌握uv包管理器，用于快速依赖管理和虚拟环境 |

### JavaScript/TypeScript (4个技能)

| Skill | 描述 |
|-------|------|
| **typescript-advanced-types** | 掌握TypeScript的高级类型系统，包括泛型和条件类型 |
| **nodejs-backend-patterns** | 构建生产就绪的Node.js服务，包括Express/Fastify和最佳实践 |
| **javascript-testing-patterns** | 实现全面的测试，包括Jest、Vitest和Testing Library |
| **modern-javascript-patterns** | 掌握ES6+特性，包括async/await、解构和函数式编程 |

### API Scaffolding (1个技能)

| Skill | 描述 |
|-------|------|
| **fastapi-templates** | 创建生产就绪的FastAPI项目，包括异步模式和错误处理 |

### Machine Learning Operations (1个技能)

| Skill | 描述 |
|-------|------|
| **ml-pipeline-workflow** | 构建端到端的MLOps管道，从数据准备到部署 |

### Security Scanning (1个技能)

| Skill | 描述 |
|-------|------|
| **sast-configuration** | 配置静态应用程序安全测试工具，用于漏洞检测 |

## Skills如何工作

### 激活

当Claude在您的请求中检测到匹配模式时，Skills会自动激活：

```
用户："设置Kubernetes部署与Helm图表"
→ 激活：helm-chart-scaffolding、k8s-manifest-generator

用户："构建用于文档问答的RAG系统"
→ 激活：rag-implementation、prompt-engineering-patterns

用户："优化Python异步性能"
→ 激活：async-python-patterns、python-performance-optimization
```

### 渐进式披露

Skills使用三层架构实现token效率：

1. **元数据** (Frontmatter)：名称和激活条件（始终加载）
2. **指令**：核心指导和模式（激活时加载）
3. **资源**：示例和模板（按需加载）

### 与Agents的集成

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

## 规范合规性

所有55个skills都遵循[Agent Skills规范](https://github.com/anthropics/skills/blob/main/agent_skills_spec.md)：

- ✓ 必需的`name`字段（连字符命名）
- ✓ 必需的`description`字段，带有"Use when"子句
- ✓ 描述少于1024个字符
- ✓ 完整、非截断的描述
- ✓ 正确的YAML frontmatter格式

## 创建新Skills

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

### Skill结构

```
plugins/{plugin-name}/
└── skills/
    └── {skill-name}/
        └── SKILL.md        # Frontmatter + 内容
```

## 优势

- **Token效率**：仅在需要时加载相关知识
- **专业化的专业知识**：深度领域知识而无冗余
- **清晰激活**：明确的触发器防止不必要的调用
- **可组合性**：跨工作流混合和匹配skills
- **可维护性**：隔离更新不影响其他skills

## 资源

- [Anthropic Skills仓库](https://github.com/anthropics/skills)
- [Agent Skills文档](https://docs.claude.com/en/docs/claude-code/skills)

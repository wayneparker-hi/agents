# Agent Reference

所有**85个专业AI agents**的完整参考，按类别组织并分配模型。

## Agent类别

### 架构与系统设计

#### 核心架构

| Agent | 模型 | 描述 |
|-------|------|------|
| [backend-architect](../plugins/backend-development/agents/backend-architect.md) | opus | RESTful API设计、微服务边界、数据库模式 |
| [frontend-developer](../plugins/multi-platform-apps/agents/frontend-developer.md) | sonnet | React组件、响应式布局、客户端状态管理 |
| [graphql-architect](../plugins/backend-development/agents/graphql-architect.md) | opus | GraphQL模式、解析器、联合架构 |
| [architect-reviewer](../plugins/comprehensive-review/agents/architect-review.md) | opus | 架构一致性分析和模式验证 |
| [cloud-architect](../plugins/cloud-infrastructure/agents/cloud-architect.md) | opus | AWS/Azure/GCP基础设施设计和成本优化 |
| [hybrid-cloud-architect](../plugins/cloud-infrastructure/agents/hybrid-cloud-architect.md) | opus | 跨云和本地环境的多云策略 |
| [kubernetes-architect](../plugins/kubernetes-operations/agents/kubernetes-architect.md) | opus | 使用Kubernetes和GitOps的云原生基础设施 |

#### UI/UX与移动

| Agent | 模型 | 描述 |
|-------|------|------|
| [ui-ux-designer](../plugins/multi-platform-apps/agents/ui-ux-designer.md) | sonnet | 界面设计、线框图、设计系统 |
| [ui-visual-validator](../plugins/accessibility-compliance/agents/ui-visual-validator.md) | sonnet | 视觉回归测试和UI验证 |
| [mobile-developer](../plugins/multi-platform-apps/agents/mobile-developer.md) | sonnet | React Native和Flutter应用程序开发 |
| [ios-developer](../plugins/multi-platform-apps/agents/ios-developer.md) | sonnet | 使用Swift/SwiftUI的原生iOS开发 |
| [flutter-expert](../plugins/multi-platform-apps/agents/flutter-expert.md) | sonnet | 高级Flutter开发与状态管理 |

### 编程语言

#### 系统与底层

| Agent | 模型 | 描述 |
|-------|------|------|
| [c-pro](../plugins/systems-programming/agents/c-pro.md) | sonnet | 系统编程，包括内存管理和操作系统接口 |
| [cpp-pro](../plugins/systems-programming/agents/cpp-pro.md) | sonnet | 现代C++，包括RAII、智能指针、STL算法 |
| [rust-pro](../plugins/systems-programming/agents/rust-pro.md) | sonnet | 内存安全的系统编程，包括所有权模式 |
| [golang-pro](../plugins/systems-programming/agents/golang-pro.md) | sonnet | 使用goroutines和channels的并发编程 |

#### Web与应用

| Agent | 模型 | 描述 |
|-------|------|------|
| [javascript-pro](../plugins/javascript-typescript/agents/javascript-pro.md) | sonnet | 现代JavaScript，包括ES6+、异步模式、Node.js |
| [typescript-pro](../plugins/javascript-typescript/agents/typescript-pro.md) | sonnet | 高级TypeScript，包括类型系统和泛型 |
| [python-pro](../plugins/python-development/agents/python-pro.md) | sonnet | Python开发，包括高级特性和优化 |
| [ruby-pro](../plugins/web-scripting/agents/ruby-pro.md) | sonnet | Ruby，包括元编程、Rails模式、gem开发 |
| [php-pro](../plugins/web-scripting/agents/php-pro.md) | sonnet | 现代PHP，包括框架和性能优化 |

#### 企业与JVM

| Agent | 模型 | 描述 |
|-------|------|------|
| [java-pro](../plugins/jvm-languages/agents/java-pro.md) | sonnet | 现代Java，包括流、并发、JVM优化 |
| [scala-pro](../plugins/jvm-languages/agents/scala-pro.md) | sonnet | 企业级Scala，包括函数式编程和分布式系统 |
| [csharp-pro](../plugins/jvm-languages/agents/csharp-pro.md) | sonnet | C#开发，包括.NET框架和模式 |

#### 专业化平台

| Agent | 模型 | 描述 |
|-------|------|------|
| [elixir-pro](../plugins/functional-programming/agents/elixir-pro.md) | sonnet | Elixir，包括OTP模式和Phoenix框架 |
| [django-pro](../plugins/api-scaffolding/agents/django-pro.md) | sonnet | Django开发，包括ORM和异步视图 |
| [fastapi-pro](../plugins/api-scaffolding/agents/fastapi-pro.md) | sonnet | FastAPI，包括异步模式和Pydantic |
| [unity-developer](../plugins/game-development/agents/unity-developer.md) | sonnet | Unity游戏开发和优化 |
| [minecraft-bukkit-pro](../plugins/game-development/agents/minecraft-bukkit-pro.md) | sonnet | Minecraft服务器插件开发 |
| [sql-pro](../plugins/database-design/agents/sql-pro.md) | sonnet | 复杂SQL查询和数据库优化 |

### 基础设施与运维

#### DevOps与部署

| Agent | 模型 | 描述 |
|-------|------|------|
| [devops-troubleshooter](../plugins/incident-response/agents/devops-troubleshooter.md) | sonnet | 生产调试、日志分析、部署故障排除 |
| [deployment-engineer](../plugins/cloud-infrastructure/agents/deployment-engineer.md) | sonnet | CI/CD管道、容器化、云部署 |
| [terraform-specialist](../plugins/cloud-infrastructure/agents/terraform-specialist.md) | sonnet | 基础设施即代码，包括Terraform模块和状态管理 |
| [dx-optimizer](../plugins/team-collaboration/agents/dx-optimizer.md) | sonnet | 开发者体验优化和工具改进 |

#### 数据库管理

| Agent | 模型 | 描述 |
|-------|------|------|
| [database-optimizer](../plugins/observability-monitoring/agents/database-optimizer.md) | sonnet | 查询优化、索引设计、迁移策略 |
| [database-admin](../plugins/database-migrations/agents/database-admin.md) | sonnet | 数据库操作、备份、复制、监控 |
| [database-architect](../plugins/database-design/agents/database-architect.md) | opus | 从零开始的数据库设计、技术选择、模式建模 |

#### 事件响应与网络

| Agent | 模型 | 描述 |
|-------|------|------|
| [incident-responder](../plugins/incident-response/agents/incident-responder.md) | opus | 生产事件管理和解决 |
| [network-engineer](../plugins/observability-monitoring/agents/network-engineer.md) | sonnet | 网络调试、负载均衡、流量分析 |

### 质量保证与安全

#### 代码质量与审查

| Agent | 模型 | 描述 |
|-------|------|------|
| [code-reviewer](../plugins/comprehensive-review/agents/code-reviewer.md) | opus | 代码审查，重点关注安全性和生产可靠性 |
| [security-auditor](../plugins/comprehensive-review/agents/security-auditor.md) | opus | 漏洞评估和OWASP合规性 |
| [backend-security-coder](../plugins/data-validation-suite/agents/backend-security-coder.md) | opus | 安全后端编码实践、API安全实现 |
| [frontend-security-coder](../plugins/frontend-mobile-security/agents/frontend-security-coder.md) | opus | XSS预防、CSP实现、客户端安全 |
| [mobile-security-coder](../plugins/frontend-mobile-security/agents/mobile-security-coder.md) | opus | 移动安全模式、WebView安全、生物识别认证 |

#### 测试与调试

| Agent | 模型 | 描述 |
|-------|------|------|
| [test-automator](../plugins/codebase-cleanup/agents/test-automator.md) | sonnet | 全面测试套件创建（单元、集成、端到端） |
| [tdd-orchestrator](../plugins/backend-development/agents/tdd-orchestrator.md) | sonnet | 测试驱动开发方法论指导 |
| [debugger](../plugins/error-debugging/agents/debugger.md) | sonnet | 错误解决和测试失败分析 |
| [error-detective](../plugins/error-debugging/agents/error-detective.md) | sonnet | 日志分析和错误模式识别 |

#### 性能与可观测性

| Agent | 模型 | 描述 |
|-------|------|------|
| [performance-engineer](../plugins/observability-monitoring/agents/performance-engineer.md) | opus | 应用程序分析和优化 |
| [observability-engineer](../plugins/observability-monitoring/agents/observability-engineer.md) | opus | 生产监控、分布式跟踪、SLI/SLO管理 |
| [search-specialist](../plugins/content-marketing/agents/search-specialist.md) | haiku | 高级网络研究和信息综合 |

### 数据与AI

#### 数据工程与分析

| Agent | 模型 | 描述 |
|-------|------|------|
| [data-scientist](../plugins/machine-learning-ops/agents/data-scientist.md) | opus | 数据分析、SQL查询、BigQuery操作 |
| [data-engineer](../plugins/data-engineering/agents/data-engineer.md) | sonnet | ETL管道、数据仓库、流式架构 |

#### 机器学习与AI

| Agent | 模型 | 描述 |
|-------|------|------|
| [ai-engineer](../plugins/llm-application-dev/agents/ai-engineer.md) | opus | LLM应用程序、RAG系统、提示管道 |
| [ml-engineer](../plugins/machine-learning-ops/agents/ml-engineer.md) | opus | ML管道、模型服务、特征工程 |
| [mlops-engineer](../plugins/machine-learning-ops/agents/mlops-engineer.md) | opus | ML基础设施、实验跟踪、模型注册表 |
| [prompt-engineer](../plugins/llm-application-dev/agents/prompt-engineer.md) | opus | LLM提示优化和工程 |

### 文档与技术写作

| Agent | 模型 | 描述 |
|-------|------|------|
| [docs-architect](../plugins/code-documentation/agents/docs-architect.md) | opus | 全面的技术文档生成 |
| [api-documenter](../plugins/api-testing-observability/agents/api-documenter.md) | sonnet | OpenAPI/Swagger规范和开发者文档 |
| [reference-builder](../plugins/documentation-generation/agents/reference-builder.md) | haiku | 技术参考和API文档 |
| [tutorial-engineer](../plugins/code-documentation/agents/tutorial-engineer.md) | sonnet | 分步教程和教育内容 |
| [mermaid-expert](../plugins/documentation-generation/agents/mermaid-expert.md) | sonnet | 图表创建（流程图、序列图、ERDs） |

### 业务与运营

#### 业务分析与金融

| Agent | 模型 | 描述 |
|-------|------|------|
| [business-analyst](../plugins/business-analytics/agents/business-analyst.md) | sonnet | 指标分析、报告、KPI跟踪 |
| [quant-analyst](../plugins/quantitative-trading/agents/quant-analyst.md) | opus | 金融建模、交易策略、市场分析 |
| [risk-manager](../plugins/quantitative-trading/agents/risk-manager.md) | sonnet | 投资组合风险监控和管理 |

#### 营销与销售

| Agent | 模型 | 描述 |
|-------|------|------|
| [content-marketer](../plugins/content-marketing/agents/content-marketer.md) | sonnet | 博客文章、社交媒体、电子邮件营销 |
| [sales-automator](../plugins/customer-sales-automation/agents/sales-automator.md) | haiku | 冷邮件、跟进、提案生成 |

#### 支持与法律

| Agent | 模型 | 描述 |
|-------|------|------|
| [customer-support](../plugins/customer-sales-automation/agents/customer-support.md) | sonnet | 支持工单、FAQ响应、客户沟通 |
| [hr-pro](../plugins/hr-legal-compliance/agents/hr-pro.md) | opus | 人力资源运营、政策、员工关系 |
| [legal-advisor](../plugins/hr-legal-compliance/agents/legal-advisor.md) | opus | 隐私政策、服务条款、法律文档 |

### SEO与内容优化

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

### 专业化领域

| Agent | 模型 | 描述 |
|-------|------|------|
| [arm-cortex-expert](../plugins/arm-cortex-microcontrollers/agents/arm-cortex-expert.md) | sonnet | ARM Cortex-M固件和外设驱动开发 |
| [blockchain-developer](../plugins/blockchain-web3/agents/blockchain-developer.md) | sonnet | Web3应用、智能合约、DeFi协议 |
| [payment-integration](../plugins/payment-processing/agents/payment-integration.md) | sonnet | 支付处理器集成（Stripe、PayPal） |
| [legacy-modernizer](../plugins/framework-migration/agents/legacy-modernizer.md) | sonnet | 遗留代码重构和现代化 |
| [context-manager](../plugins/agent-orchestration/agents/context-manager.md) | haiku | 多代理上下文管理 |

## 模型配置

Agents根据任务复杂性和计算需求分配给特定的Claude模型。

### 模型分布摘要

| 模型 | Agent数量 | 使用场景 |
|-------|-------------|----------|
| Haiku | 47 | 快速执行任务：测试、文档、运维、数据库优化、业务 |
| Sonnet | 97 | 复杂推理、架构、语言专业知识、编排、安全 |

### 模型选择标准

#### Haiku - 快速执行与确定性任务

**使用时机：**
- 根据明确定义的规范生成代码
- 遵循既定模式创建测试
- 使用清晰模板编写文档
- 执行基础设施操作
- 执行数据库查询优化
- 处理客户支持响应
- 处理SEO优化任务
- 管理部署管道

#### Sonnet - 复杂推理与架构

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

## Agent调用

### 自然语言

可以通过自然语言调用Agents，当您需要Claude推理使用哪个专家时：

```
"使用backend-architect设计认证API"
"让security-auditor扫描OWASP漏洞"
"让performance-engineer优化这个数据库查询"
```

### 斜杠命令

许多agents可通过插件斜杠命令直接调用：

```bash
/backend-development:feature-development 用户认证
/security-scanning:security-sast
/incident-response:smart-fix "支付服务中的内存泄漏"
```

## 贡献

要添加新agent：

1. 创建`plugins/{plugin-name}/agents/{agent-name}.md`
2. 添加包含名称、描述和模型分配的frontmatter
3. 编写全面的系统提示
4. 更新`.claude-plugin/marketplace.json`中的插件定义

详情请参见[贡献指南](../CONTRIBUTING.md)。
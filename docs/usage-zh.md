# Usage Guide

使用agents、斜杠命令和多代理工作流的完整指南。

## 概述

插件生态系统提供两个主要接口：

1. **斜杠命令** - 直接调用工具和工作流
2. **自然语言** - Claude推理使用哪个agents

## 斜杠命令

斜杠命令是使用agents和工作流的主要接口。每个插件提供命名空间命令，您可以直接运行。

### 命令格式

```bash
/plugin-name:command-name [参数]
```

### 发现命令

列出所有来自已安装插件的斜杠命令：

```bash
/plugin
```

### 斜杠命令的优势

- **直接调用** - 无需用自然语言描述您想要什么
- **结构化参数** - 显式传递参数以实现精确控制
- **可组合性** - 链接命令以实现复杂工作流
- **可发现性** - 使用`/plugin`查看所有可用命令

## 自然语言

Agents也可以通过自然语言调用，当您需要Claude推理使用哪个专家时：

```
"使用backend-architect设计认证API"
"让security-auditor扫描OWASP漏洞"
"让performance-engineer优化这个数据库查询"
```

Claude Code根据您的请求自动选择和协调适当的agents。

## 按类别分类的命令参考

### 开发与功能

| 命令 | 描述 |
|---------|-------------|
| `/backend-development:feature-development` | 端到端后端功能开发 |
| `/full-stack-orchestration:full-stack-feature` | 完整的全栈功能实现 |
| `/multi-platform-apps:multi-platform` | 跨平台应用开发协调 |

### 测试与质量

| 命令 | 描述 |
|---------|-------------|
| `/unit-testing:test-generate` | 生成全面的单元测试 |
| `/tdd-workflows:tdd-cycle` | 完整的TDD红绿重构周期 |
| `/tdd-workflows:tdd-red` | 首先编写失败测试 |
| `/tdd-workflows:tdd-green` | 实现代码以通过测试 |
| `/tdd-workflows:tdd-refactor` | 重构通过的测试 |

### 代码质量与审查

| 命令 | 描述 |
|---------|-------------|
| `/code-review-ai:ai-review` | AI驱动的代码审查 |
| `/comprehensive-review:full-review` | 多视角分析 |
| `/comprehensive-review:pr-enhance` | 增强拉取请求 |

### 调试与故障排除

| 命令 | 描述 |
|---------|-------------|
| `/debugging-toolkit:smart-debug` | 交互式智能调试 |
| `/incident-response:incident-response` | 生产事件管理 |
| `/incident-response:smart-fix` | 自动化事件解决 |
| `/error-debugging:error-analysis` | 深度错误分析 |
| `/error-debugging:error-trace` | 堆栈跟踪调试 |
| `/error-diagnostics:smart-debug` | 智能诊断调试 |
| `/distributed-debugging:debug-trace` | 分布式系统跟踪 |

### 安全

| 命令 | 描述 |
|---------|-------------|
| `/security-scanning:security-hardening` | 全面的安全加固 |
| `/security-scanning:security-sast` | 静态应用程序安全测试 |
| `/security-scanning:security-dependencies` | 依赖漏洞扫描 |
| `/security-compliance:compliance-check` | SOC2/HIPAA/GDPR合规 |
| `/frontend-mobile-security:xss-scan` | XSS漏洞扫描 |

### 基础设施与部署

| 命令 | 描述 |
|---------|-------------|
| `/observability-monitoring:monitor-setup` | 设置监控基础设施 |
| `/observability-monitoring:slo-implement` | 实现SLO/SLI指标 |
| `/deployment-validation:config-validate` | 部署前验证 |
| `/cicd-automation:workflow-automate` | CI/CD管道自动化 |

### 数据与ML

| 命令 | 描述 |
|---------|-------------|
| `/machine-learning-ops:ml-pipeline` | ML训练管道编排 |
| `/data-engineering:data-pipeline` | ETL/ELT管道构建 |
| `/data-engineering:data-driven-feature` | 数据驱动功能开发 |

### 文档

| 命令 | 描述 |
|---------|-------------|
| `/code-documentation:doc-generate` | 生成全面的文档 |
| `/code-documentation:code-explain` | 解释代码功能 |
| `/documentation-generation:doc-generate` | OpenAPI规范、图表、教程 |

### 重构与维护

| 命令 | 描述 |
|---------|-------------|
| `/code-refactoring:refactor-clean` | 代码清理和重构 |
| `/code-refactoring:tech-debt` | 技术债务管理 |
| `/codebase-cleanup:deps-audit` | 依赖审计 |
| `/codebase-cleanup:tech-debt` | 技术债务减少 |
| `/framework-migration:legacy-modernize` | 遗留代码现代化 |
| `/framework-migration:code-migrate` | 框架迁移 |
| `/framework-migration:deps-upgrade` | 依赖升级 |

### 数据库

| 命令 | 描述 |
|---------|-------------|
| `/database-migrations:sql-migrations` | SQL迁移自动化 |
| `/database-migrations:migration-observability` | 迁移监控 |
| `/database-cloud-optimization:cost-optimize` | 数据库和云优化 |

### Git与PR工作流

| 命令 | 描述 |
|---------|-------------|
| `/git-pr-workflows:pr-enhance` | 增强拉取请求质量 |
| `/git-pr-workflows:onboard` | 团队入职自动化 |
| `/git-pr-workflows:git-workflow` | Git工作流自动化 |

### 项目脚手架

| 命令 | 描述 |
|---------|-------------|
| `/python-development:python-scaffold` | FastAPI/Django项目设置 |
| `/javascript-typescript:typescript-scaffold` | Next.js/React + Vite设置 |
| `/systems-programming:rust-project` | Rust项目脚手架 |

### AI与LLM开发

| 命令 | 描述 |
|---------|-------------|
| `/llm-application-dev:langchain-agent` | LangChain agent开发 |
| `/llm-application-dev:ai-assistant` | AI助手实现 |
| `/llm-application-dev:prompt-optimize` | 提示工程优化 |
| `/agent-orchestration:multi-agent-optimize` | 多代理优化 |
| `/agent-orchestration:improve-agent` | Agent改进工作流 |

### 测试与性能

| 命令 | 描述 |
|---------|-------------|
| `/performance-testing-review:ai-review` | 性能分析 |
| `/application-performance:performance-optimization` | 应用优化 |

### 团队协作

| 命令 | 描述 |
|---------|-------------|
| `/team-collaboration:issue` | 问题管理自动化 |
| `/team-collaboration:standup-notes` | 站会笔记生成 |

### 无障碍

| 命令 | 描述 |
|---------|-------------|
| `/accessibility-compliance:accessibility-audit` | WCAG合规审计 |

### API开发

| 命令 | 描述 |
|---------|-------------|
| `/api-testing-observability:api-mock` | API模拟和测试 |

### 上下文管理

| 命令 | 描述 |
|---------|-------------|
| `/context-management:context-save` | 保存对话上下文 |
| `/context-management:context-restore` | 恢复先前上下文 |

## 多代理工作流示例

插件提供预配置的多代理工作流，可通过斜杠命令访问。

### 全栈开发

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

### 安全加固

```bash
# 全面的安全评估和修复
/security-scanning:security-hardening --level comprehensive

# 自然语言替代
"执行安全审计并实施OWASP最佳实践"
```

**编排：** security-auditor → backend-security-coder → frontend-security-coder → mobile-security-coder → test-automator

### 数据/ML管道

```bash
# ML功能开发与生产部署
/machine-learning-ops:ml-pipeline "客户流失预测模型"

# 自然语言替代
"构建客户流失预测模型与部署"
```

**编排：** data-scientist → data-engineer → ml-engineer → mlops-engineer → performance-engineer

### 事件响应

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

## 结合自然语言和命令

您可以混合两种方法以实现最佳灵活性：

```
# 从命令开始进行结构化工作流
/full-stack-orchestration:full-stack-feature "支付处理"

# 然后提供自然语言指导
"确保PCI-DSS合规并集成Stripe"
"添加失败事务的重试逻辑"
"设置欺诈检测规则"
```

## 最佳实践

### 何时使用斜杠命令

- **结构化工作流** - 多步骤过程，有明确阶段
- **重复任务** - 频繁执行的操作
- **精确控制** - 需要特定参数时
- **发现** - 探索可用功能

### 何时使用自然语言

- **探索性工作** - 不确定使用哪个工具时
- **复杂推理** - Claude需要协调多个agents时
- **情境决策** - 正确方法取决于情况时
- **临时任务** - 不符合命令的一次性操作

### 工作流组合

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

## Agent Skills集成

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

## 参见

- [Agent Skills](./agent-skills.md) - 专业化的知识包
- [Agent Reference](./agents.md) - 完整agent目录
- [Plugin Reference](./plugins.md) - 所有63个插件
- [Architecture](./architecture.md) - 设计原则
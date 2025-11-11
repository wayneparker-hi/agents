# DDD Plugin 完整创建计划

## 一、Plugin 结构概览

```
plugins/domain-driven-design/
├── agents/           # 4 个专业化 agents
├── commands/         # 3 个工作流 commands
└── skills/           # 8 个知识模块 skills
```

## 二、组件清单（共 15 个组件）

### Agents（4 个）
1. **strategic-designer.md** - 战略设计专家（限界上下文、上下文映射、子域划分）
2. **domain-modeler.md** - 领域建模专家（聚合、实体、值对象、领域服务）
3. **architecture-advisor.md** - 架构顾问（分层架构、CQRS、事件溯源、六边形架构）
4. **ubiquitous-language-facilitator.md** - 统一语言促进者（团队沟通、术语表管理）

### Commands（3 个）
1. **bounded-context-mapping.md** - 限界上下文识别与映射工作流（9 阶段）
2. **domain-modeling-workflow.md** - 领域建模完整工作流（事件风暴 → 建模 → 实现，11 阶段）
3. **ddd-refactoring.md** - 遗留系统 DDD 重构工作流（评估 → 改造 → 验证，8 阶段）

### Skills（8 个）
1. **bounded-context-design/** - 限界上下文设计（识别方法、控制力、四个自治要素）
2. **context-mapping-patterns/** - 上下文映射模式（团队协作模式、通信集成模式）
3. **aggregate-design-principles/** - 聚合设计原则（边界识别、事务一致性、根实体设计）
4. **entity-value-object-patterns/** - 实体和值对象模式（标识设计、不变性、领域行为）
5. **domain-events/** - 领域事件（事件设计、发布订阅、事件溯源基础）
6. **event-storming/** - 事件风暴方法论（完整步骤、产物交付、协作技巧）
7. **layered-architecture/** - 分层架构（DDD 四层架构、依赖规则、防腐层）
8. **ubiquitous-language/** - 统一语言建立（提取方法、验证机制、演化管理）

## 三、内容整合来源

### 战略设计核心概念（从专栏文章直接提取并整合）
- **限界上下文**：定义、识别方法、业务边界、团队边界、技术边界、四个自治要素（最小完备、稳定空间、自我履行、独立进化）
- **上下文映射**：合作关系、共享内核、客户-供应商、遵奉者、防腐层、开放主机服务、发布语言、各行其道
- **统一语言**：场景分析、术语提取、领域词汇表、团队沟通、持续演化
- **分层架构**：用户接口层、应用层、领域层、基础设施层、依赖倒置、六边形架构

### 战术设计核心模式（从专栏文章直接提取并整合）
- **实体**：身份标识（通用类型与领域类型）、属性设计（基本属性与组合属性）、领域行为（变更状态、自给自足、互为协作）
- **值对象**：不变性、等值性、自描述性、业务规则封装
- **聚合**：聚合根、事务边界、不变量保护、聚合间引用规则、聚合大小设计
- **领域服务**：无状态操作、跨聚合协调、领域计算
- **领域事件**：事件定义、命名约定、事件存储、订阅机制
- **事件风暴**：橙色便签（领域事件）、蓝色便签（命令）、黄色便签（聚合）、粉色便签（外部系统）

### 架构与实践方法（从专栏文章直接提取并整合）
- **CQRS 模式**：命令查询分离、读写模型、最终一致性
- **事件溯源**：事件存储、状态重建、快照机制
- **DCI 模式**：Data-Context-Interaction、角色注入
- **四色建模法**：时标型对象、描述型对象、角色型对象、时刻-地点型对象

## 四、实施步骤

### 阶段 1：创建 Plugin 基础结构
1. 创建 `plugins/domain-driven-design/` 目录
2. 创建 `agents/`、`commands/`、`skills/` 子目录
3. 每个 skill 创建对应的子目录结构（SKILL.md、references/、assets/）

### 阶段 2：创建 Agents（优先级：高）
按顺序创建 4 个 agent 文件（model: sonnet）：

1. **strategic-designer.md**（约 300 行）
   - 从专栏提取：限界上下文识别、上下文映射、子域划分方法
   - 包含：战略设计决策流程、团队协作模式、架构视图输出

2. **domain-modeler.md**（约 350 行）
   - 从专栏提取：聚合设计原则、实体/值对象模式、领域服务设计
   - 包含：建模步骤、代码结构建议、测试策略

3. **architecture-advisor.md**（约 280 行）
   - 从专栏提取：分层架构、CQRS、事件溯源、六边形架构
   - 包含：架构决策指南、技术选型建议、迁移路径

4. **ubiquitous-language-facilitator.md**（约 200 行）
   - 从专栏提取：统一语言建立方法、术语表管理、团队沟通实践
   - 包含：词汇提取工作坊、验证检查清单、演化追踪

每个 agent 结构：
- Frontmatter（name、description、model）
- Purpose & Core Philosophy
- Core Capabilities（3-5 个详细分类）
- Workflow Position（与其他 agents 的协作关系）
- Response Approach（分步骤的响应方法）
- Example Interactions（中文使用场景示例）
- Output Examples（预期输出格式）

### 阶段 3：创建 Skills（优先级：高）
按优先级创建 8 个 skill 目录：

**优先级 1（核心战略战术）：**

1. **bounded-context-design/**
   - `SKILL.md`（300 行）：限界上下文定义、识别步骤、控制力三要素、自治要素
   - `references/context-identification-guide.md`：详细识别指南
   - `assets/context-canvas-template.md`：限界上下文画布模板

2. **aggregate-design-principles/**
   - `SKILL.md`（350 行）：聚合概念、设计原则、边界识别、一致性规则
   - `references/aggregate-patterns.md`：常见聚合模式
   - `assets/aggregate-design-checklist.md`：聚合设计检查清单

3. **event-storming/**
   - `SKILL.md`（280 行）：事件风暴步骤、便签颜色含义、协作技巧
   - `references/event-storming-guide.md`：完整实施指南
   - `assets/event-storming-canvas.md`：事件风暴画布

**优先级 2（补充战术模式）：**

4. **entity-value-object-patterns/**
   - `SKILL.md`（300 行）：实体与值对象定义、标识设计、行为设计
   - `references/identity-design-guide.md`：标识设计指南
   - `assets/value-object-examples.md`：常见值对象示例

5. **context-mapping-patterns/**
   - `SKILL.md`（250 行）：上下文映射关系、协作模式、集成方式
   - `references/integration-patterns.md`：集成模式详解
   - `assets/context-map-template.md`：上下文地图模板

6. **domain-events/**
   - `SKILL.md`（280 行）：领域事件设计、命名约定、发布订阅机制
   - `references/event-sourcing-basics.md`：事件溯源基础
   - `assets/event-schema-template.md`：事件模式模板

**优先级 3（架构与实践）：**

7. **ubiquitous-language/**
   - `SKILL.md`（220 行）：统一语言建立步骤、术语表管理、验证方法
   - `references/terminology-extraction.md`：术语提取方法
   - `assets/glossary-template.md`：术语表模板

8. **layered-architecture/**
   - `SKILL.md`（260 行）：DDD 四层架构、依赖规则、防腐层设计
   - `references/architecture-evolution.md`：架构演进路径
   - `assets/layer-checklist.md`：分层检查清单

每个 skill 结构：
- Frontmatter（name、description）
- When to Use This Skill（中文触发场景）
- Core Concepts（核心概念详解）
- Patterns & Practices（模式和实践，带代码示例）
- Best Practices（最佳实践列表）
- Common Pitfalls（常见陷阱和反模式）
- Resources（引用本地 references 和 assets）

### 阶段 4：创建 Commands（优先级：中）
创建 3 个 command 文件（每个 150-200 行）：

1. **bounded-context-mapping.md**
   - 9 阶段工作流：需求收集 → 场景分析 → 事件识别 → 领域建模 → 边界划分 → 映射关系 → 团队协作模式 → 技术边界 → 输出文档
   - 使用 Task tool 调用 strategic-designer 和 ubiquitous-language-facilitator
   - 参数：`--domain`、`--team-structure`、`--complexity`
   - 输出：限界上下文图、上下文地图、术语表

2. **domain-modeling-workflow.md**
   - 11 阶段工作流：事件风暴 → 命令识别 → 聚合识别 → 聚合设计 → 实体建模 → 值对象提取 → 领域服务 → 领域事件 → 资源库设计 → 应用服务 → 测试设计
   - 使用 Task tool 调用 domain-modeler 和相关 skills
   - 参数：`--subdomain`、`--modeling-method`（event-storming/four-color）、`--tech-stack`
   - 输出：领域模型图、代码骨架、测试用例

3. **ddd-refactoring.md**
   - 8 阶段工作流：现状评估 → 识别问题 → 划分边界 → 提取聚合 → 建立防腐层 → 渐进迁移 → 验证一致性 → 团队培训
   - 使用 Task tool 调用 strategic-designer、domain-modeler、architecture-advisor
   - 参数：`--legacy-type`（monolith/distributed）、`--migration-strategy`（big-bang/strangler）
   - 输出：重构路线图、迁移计划、风险评估

每个 command 结构：
- 工作流目标描述（中文）
- Extended thinking（编排逻辑、复杂度处理）
- Configuration Options（配置选项，中文说明）
- Phase 1-N（分阶段步骤，使用 Task tool）
- Execution Parameters（必需/可选参数）
- Success Criteria（成功标准）
- Rollback Strategy（回滚策略）

### 阶段 5：内容提取与整合
从专栏文章中直接复制并整合内容到相应组件。

### 阶段 6：创建资源文件
为每个 skill 创建详细的 references 和 assets。

### 阶段 7：测试与优化
1. 验证所有文件的 frontmatter 格式符合规范
2. 确保 agent description 包含 "Use PROACTIVELY when" 触发条件
3. 验证 skill description 包含 "Use when" 激活场景
4. 测试 command 工作流的 Task tool 调用链
5. 确认中文使用场景示例的准确性和实用性
6. 检查 agent 之间的协作关系清晰度

### 阶段 8：文档编写
创建并写入 docs/ 目录。

## 五、预期成果

### 文件统计
- 4 个 agent 文件（~1,100 行）
- 3 个 command 文件（~500 行）
- 8 个 skill 主文件（~2,200 行）
- ~24 个 reference 文档（~4,000 行）
- ~10 个 asset 文件（~800 行）
- 1 个 plugin 文档（~300 行）
- **总计：~8,900 行**

### 覆盖的知识范围
- **战略设计**：限界上下文、上下文映射、子域划分、统一语言、架构愿景
- **战术设计**：聚合、实体、值对象、领域服务、领域事件、资源库、工厂、规格
- **建模方法**：事件风暴、四色建模、场景驱动设计
- **架构模式**：分层架构、六边形架构、CQRS、事件溯源、DCI
- **实践指南**：团队协作、遗留系统重构、测试策略、持续演化

## 六、使用场景示例（中文）

创建完成后，用户可以这样使用：

### 场景 1：新项目的战略设计
```
用户："我要设计一个电商系统的限界上下文"
→ 自动激活 strategic-designer agent
→ 可选运行：/bounded-context-mapping --domain=电商 --complexity=中等
→ 输出：限界上下文图、上下文映射关系、术语表
```

### 场景 2：聚合设计
```
用户："帮我设计 Order 聚合，需要包含订单项、支付、物流信息"
→ 自动激活 domain-modeler agent
→ 自动加载 aggregate-design-principles skill
→ 输出：聚合边界分析、根实体设计、值对象建议、代码骨架
```

### 场景 3：事件风暴工作坊
```
用户："我想对订单子域做事件风暴"
→ 可选运行：/domain-modeling-workflow --subdomain=订单管理 --modeling-method=event-storming
→ 阶段 1：引导识别领域事件（激活 event-storming skill）
→ 阶段 2-11：依次进行命令识别、聚合识别、建模、实现
→ 输出：完整的领域模型和代码实现
```

### 场景 4：遗留系统重构
```
用户："我有一个单体应用，想用 DDD 重构成微服务"
→ 可选运行：/ddd-refactoring --legacy-type=monolith --migration-strategy=strangler
→ 输出：限界上下文识别、重构路线图、防腐层设计、迁移计划
```

### 场景 5：架构决策咨询
```
用户："我应该用 CQRS 还是传统的 CRUD？"
→ 自动激活 architecture-advisor agent
→ 分析：业务复杂度、读写比例、一致性要求、团队能力
→ 输出：架构建议、技术选型对比、实施路径
```

### 场景 6：统一语言建立
```
用户："团队对业务术语理解不一致，怎么办？"
→ 自动激活 ubiquitous-language-facilitator agent
→ 自动加载 ubiquitous-language skill
→ 输出：术语提取工作坊计划、术语表模板、验证检查清单
```

## 七、后续扩展规划

### 短期扩展（3-6 个月）
- 添加更多战术模式 skills：规格模式、工厂模式、资源库实现
- 创建代码生成 commands：生成 Java/C#/Python 的聚合代码骨架
- 添加更多语言的实现示例和模板

### 长期扩展（6-12 个月）
- 集成架构决策记录（ADR）工作流
- 添加领域模型可视化生成器
- 创建 DDD 健康度评估 command
- 集成测试用例自动生成（基于领域模型）

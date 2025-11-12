---
name: architectural-prototyping
description: 架构原型设计技能，指导如何使用演化型原型验证架构设计。包括演化型vs抛弃型原型、Hollow Architecture原型、性能验证和迭代原型过程
---

# 架构原型设计技能（Architectural Prototyping）

## 概述

架构原型是验证架构设计的关键技术。Philippe Kruchten在论文中明确强调使用**演化型原型**（Evolutionary Prototype）而非抛弃型原型，使原型逐渐演化成真实系统。

## 核心概念

### 演化型 vs 抛弃型原型

#### 演化型原型（Evolutionary Prototype）✅

**定义**：
> "We are speaking here of an evolutionary prototype, that slowly grows into becoming the system, and not of throw-away, exploratory prototypes."

**特征**：
- 初始架构原型逐渐演化成真实系统
- 每次迭代添加更多场景支持
- 保持架构完整性
- 生产代码的基础
- 架构决策被实际代码验证

**优势**：
- 架构与实现保持一致
- 团队通过实践学习架构
- 持续集成架构改进
- 避免抛弃原型的浪费

**何时使用**：
- 几乎所有项目
- 特别是架构复杂、不确定性高的项目
- 当需要持续验证的时候

#### 抛弃型原型（Throw-away Prototype）⚠️

**定义**：用于探索性实验，不打算演化成生产代码。

**使用场景**：
- 技术可行性验证（会放弃）
- 用户界面设计探索
- 算法性能比较（快速测试）

**风险**：
- 原型质量不足以成为生产基础
- 重复投资（要重写）
- 架构与实现脱节

**建议**：尽量避免，改用演化型原型

---

## "Hollow" Architecture 原型

### 概念

**原文引用**（Philippe Kruchten）：
> "It is also possible to implement a 'hollow' process architecture with dummy loads for the processes, and measure its performance on the target system."

**定义**：
只包含框架结构（进程、组件、通信），用虚拟负载替代真实业务逻辑的原型。

### 目的

1. **早期性能验证**
   - 在实现真实业务逻辑前验证进程架构
   - 测量通信开销和延迟
   - 识别性能瓶颈

2. **容量规划**
   - 确定需要的硬件资源
   - 验证扩展性假设
   - 优化部署配置（副本数、资源限制等）

3. **风险降低**
   - 在投入大量开发前验证架构可行性
   - 避免后期重大架构返工
   - 建立技术可行性基线

### 实现步骤

#### **Step 1: 定义进程骨架**
- 创建所有major processes的空壳
- 实现进程间通信接口
- 不包含真实业务逻辑

#### **Step 2: 添加虚拟负载**
- 使用循环或延迟模拟处理时间
- 生成预期大小的数据和消息
- 模拟数据库查询、外部API调用延迟

#### **Step 3: 部署到目标环境**
- 部署到实际硬件或云环境
- 使用真实网络连接
- 配置与生产类似的环境

#### **Step 4: 负载测试**
- 生成预期的负载模式
- 记录响应时间、吞吐量
- 监控CPU、内存、网络使用

#### **Step 5: 分析和调整**
- 识别性能瓶颈
- 调整进程数量、部署配置
- 验证非功能需求是否可满足
- 记录改进建议

### 示例：电商系统 Hollow Architecture

**架构组件**：
- API Gateway
- Order Service
- Inventory Service
- Payment Service
- Database

**代码示例（Python Flask）**：

```python
# Hollow Order Service
from flask import Flask, jsonify
import time
import random

app = Flask(__name__)

@app.route('/orders', methods=['POST'])
def create_order():
    # 模拟订单处理（50-100ms）
    time.sleep(random.uniform(0.05, 0.1))

    # 模拟调用Inventory Service（网络延迟）
    time.sleep(0.02)

    # 模拟数据库写入（10ms）
    time.sleep(0.01)

    # 返回模拟响应
    return jsonify({'orderId': random.randint(1000, 9999)}), 201

if __name__ == '__main__':
    app.run(port=8001)
```

**负载测试（K6）**：

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export let options = {
  vus: 100,        // 100个虚拟用户
  duration: '30s', // 运行30秒
};

export default function () {
  let response = http.post('http://localhost:8001/orders', JSON.stringify({
    items: [{productId: 1, quantity: 2}]
  }), {
    headers: { 'Content-Type': 'application/json' },
  });

  check(response, {
    'status is 201': (r) => r.status === 201,
    'response time < 200ms': (r) => r.timings.duration < 200,
  });

  sleep(1);
}
```

**度量指标**：
- 平均响应时间
- p95、p99响应时间
- 吞吐量（requests per second）
- 错误率
- 资源使用（CPU、内存、网络）

### 现代等价物

#### 1. 容器化性能测试框架

**工具**：
- Docker + docker-compose：本地模拟完整架构
- Kubernetes + Minikube：模拟真实K8s环境
- LocalStack：AWS服务本地模拟

**优势**：
- 接近生产环境
- 可重复性好
- 易于扩展

#### 2. 服务网格虚拟负载

**工具**：Istio, Linkerd with VirtualService

**用法**：
- 定义虚拟服务，返回模拟响应
- 注入延迟和故障
- 验证弹性特性（熔断、重试等）

#### 3. 性能测试框架

**常用工具**：
- **JMeter**：Java生态标准
- **Gatling**：Scala，更先进的脚本
- **K6**：JavaScript，Prometheus集成
- **Locust**：Python，易定制

---

## 迭代原型过程

### 初始阶段（Iteration 0）

**目标**：建立基础架构原型

**内容**：
- Strawman architecture（草稿架构）
- 少量关键场景（2-3个）
- 基本的5个视图
- 简单的hollow实现

**活动**：
1. 基于风险和覆盖面选择2-3个关键场景
2. 设计草稿架构（strawman）
3. 脚本化场景以识别主要抽象
4. 布局到4个蓝图（逻辑、开发、进程、物理）
5. 实现hollow架构
6. 测试并度量
7. 捕获经验教训

**持续时间**：
- 小项目：2-3周
- 大项目：6-9个月

### Iteration 1-3：扩展和稳定

**每次迭代的过程**：

1. **重新评估风险**
   - 哪些风险已经降低
   - 哪些新的风险出现

2. **扩展场景集**
   - 选择额外场景（基于风险和覆盖）
   - 在现有架构中脚本化新场景
   - 发现新的架构元素或需要大变更

3. **更新5个视图**
   - 修改逻辑视图（新类、新机制）
   - 修改开发视图（新包、新分层）
   - 修改进程视图（新任务、新通信）
   - 修改物理视图（新部署配置）
   - 修改场景视图（细化场景）

4. **升级原型实现**
   - 添加新的hollow服务
   - 实现新的通信路径
   - 保持架构骨架稳定

5. **测试和度量**
   - 负载测试
   - 性能分析
   - 识别新的瓶颈

6. **架构评审**
   - 评审设计的简洁性
   - 识别重用机会
   - 寻找通用机制

7. **更新文档**
   - 设计指南
   - 理由和约束
   - 决策记录

8. **捕获经验教训**
   - 什么工作良好
   - 什么需要改进
   - 团队学到了什么

### 稳定标准

迭代结束时，当满足以下条件时，架构已经稳定：

- ✅ 2-3次迭代后不再发现新的主要抽象
- ✅ 不再发现新的major tasks或进程
- ✅ 不再发现新的接口或通信模式
- ✅ 所有关键场景都能被支持
- ✅ 架构团队对设计有共识

---

## 最佳实践

### DO's ✅

1. **从第一天开始编码架构原型**
   - 不要只写文档
   - 代码验证设计
   - 早期发现问题

2. **每次迭代都部署和测试**
   - 在目标环境测试
   - 度量性能
   - 验证非功能需求

3. **使用真实的通信机制**
   - HTTP/gRPC，不要只是方法调用
   - 真实的消息队列，不要内存消息
   - 真实的数据库延迟模拟

4. **度量性能和资源使用**
   - 不要凭感觉
   - 收集数据驱动改进
   - 建立性能基线

5. **持续集成架构改进**
   - 原型逐渐成长为生产系统
   - 版本控制所有代码
   - CI/CD流程

6. **让整个团队参与**
   - 开发者参与架构设计
   - 架构师参与代码实现
   - 共同拥有架构

### DON'Ts ❌

1. ❌ **不要只写文档不写代码**
   - 文档无法验证设计
   - 代码是最好的文档

2. ❌ **不要等到"架构完成"才开始实现**
   - 架构是在实践中逐渐清晰的
   - 并发进行设计和实现

3. ❌ **不要忽视非功能需求**
   - 性能不是可选的
   - 从第一个迭代就测试

4. ❌ **不要在隔离环境中原型**
   - 在目标环境测试
   - 真实网络、真实硬件

5. ❌ **不要抛弃原型然后重写**
   - 持续演化，不要重写
   - 代价太高

6. ❌ **不要假设一切顺利**
   - 考虑失败、超时、延迟
   - 测试故障场景

---

## 检查清单

### 架构原型检查清单

- [ ] 所有major processes都有hollow实现
- [ ] 虚拟负载接近预期的真实处理时间
- [ ] 通信延迟得到模拟（网络、数据库等）
- [ ] 部署环境接近生产
- [ ] 负载测试覆盖关键场景
- [ ] 所有关键性能指标都被度量
- [ ] 瓶颈已识别和分析
- [ ] 非功能需求已验证
- [ ] 架构调整已记录在案
- [ ] 代码版本受控
- [ ] CI/CD流程已建立

### 迭代完成检查清单

- [ ] 2-3个关键场景已脚本化
- [ ] 5个视图都已更新
- [ ] 架构原型已部署和测试
- [ ] 性能度量已收集
- [ ] 新的架构元素已识别
- [ ] 不再发现主要的新抽象
- [ ] 团队对设计有共识
- [ ] 经验教训已记录

---

## 工具和技术堆栈

### 架构原型工具

**容器和编排**：
- Docker：容器化hollow服务
- docker-compose：本地编排
- Kubernetes + Minikube：生产级环境模拟

**IaC工具**：
- Terraform：云基础设施定义
- Pulumi：编程式IaC
- Helm Charts：K8s包管理

**CI/CD**：
- GitHub Actions, GitLab CI, Jenkins
- 自动化部署和测试

### 性能度量工具

**负载测试**：
- K6：现代、易用、Prometheus集成
- Gatling：功能强大、Scala
- JMeter：Java生态标准

**APM和监控**：
- Prometheus + Grafana：开源标准
- Jaeger/Zipkin：分布式追踪
- ELK Stack：日志聚合

### 架构验证

**代码级验证**：
- ArchUnit（Java）：架构规则单元测试
- Dependency Cruiser（JavaScript）：依赖分析

**图表驱动**：
- PlantUML：版本控制友好的架构图
- C4 Plant UML：分层架构可视化

---

## 参考资源

- Philippe Kruchten原论文，第13页：迭代过程部分
- `agents/process-view-architect.md` - Hollow architecture原型部分
- `agents/41view-master-architect.md` - 8-12周完整工作流程
- `skills/process-view-design/SKILL.md` - 进程视图设计详细指南

---

## 常见问题

**Q: Hollow架构需要写多少代码？**
A: 通常10-20%的真实代码量。足以验证架构和性能，但不实现完整业务逻辑。

**Q: 需要多精确的虚拟负载？**
A: 需要接近真实的处理时间和数据大小。过于简化会隐藏真实的性能问题。

**Q: 如何处理数据库？**
A: 可以使用真实数据库或内存数据库（H2, SQLite）进行快速测试。

**Q: Hollow架构最后怎么办？**
A: 保留框架，逐步添加真实业务逻辑。演化成最终产品。

**Q: 何时停止Hollow架构？**
A: 当所有关键场景都已验证、非功能需求已满足、架构已稳定时。

---

**记住**：架构不只是文档，架构是可执行的、可验证的、可演化的代码。

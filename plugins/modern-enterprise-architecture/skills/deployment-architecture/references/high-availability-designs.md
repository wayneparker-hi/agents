# 高可用部署架构详解

## 概述

高可用架构的目标是最小化系统故障对用户的影响。通过冗余、自动转移、监控等技术，确保系统持续可用。

---

## 关键指标

### 可用性等级

```
99.0% (4 个 9)      → 年宕机 3.65 天     = 不可接受
99.5% (5 个 9)      → 年宕机 1.83 天     = 勉强
99.9% (6 个 9)      → 年宕机 8.77 小时   = 基本要求
99.95% (7 个 9)     → 年宕机 4.38 小时   = 不错
99.99% (8 个 9)     → 年宕机 52.6 分钟   = 严格
99.995% (9 个 9)    → 年宕机 26.3 分钟   = 非常严格
99.999% (10 个 9)   → 年宕机 2.63 分钟   = 极其严格（如支付系统）
```

### 故障恢复指标

```
RTO（Recovery Time Objective）：恢复时间目标
  ├─ 定义：从故障发生到系统恢复的最长时间
  ├─ 示例：RTO = 1 分钟（故障 1 分钟内恢复）
  ├─ 对应措施：自动故障转移、冗余、监控
  └─ 成本：RTO 越短，成本越高

RPO（Recovery Point Objective）：恢复点目标
  ├─ 定义：可以接受的最大数据丢失量
  ├─ 示例：RPO = 5 分钟（可以丢失 5 分钟的数据）
  ├─ 对应措施：数据复制频率、备份策略
  └─ 成本：RPO 越小，同步频率越高，成本越高
```

### 可用性公式

```
系统可用性 = 1 - (年宕机时间 / 365 天)

例如：
99.9% 可用性 = 年宕机 8.77 小时
  = (1 - 99.9%) × 365 × 24 小时
  = 0.001 × 8760 小时
  = 8.76 小时
```

---

## 模式 1：单机 + 自动转移（Single Instance + Failover）

### 架构

```
用户请求
  ↓
DNS（指向主实例）
  ↓
主实例（处理所有请求）
  ├─ 异步复制到 →
  └─ 备实例（待命）

故障检测：
  ├─ 监控工具持续检查主实例
  └─ 如果主实例故障 → DNS 切换指向备实例

数据：
  ├─ 主实例：主数据库
  └─ 备实例：从数据库（只读复制）
```

### 实现

**数据库主从复制**：
```sql
-- 主实例
CREATE TABLE users (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);

-- 备实例配置
CHANGE MASTER TO
  MASTER_HOST = 'primary.db.com',
  MASTER_USER = 'repl',
  MASTER_PASSWORD = 'password',
  MASTER_LOG_FILE = 'mysql-bin.000001',
  MASTER_LOG_POS = 123;

START SLAVE;
```

**故障转移脚本**：
```bash
#!/bin/bash

# 检查主实例健康状态
check_primary() {
  mysql -h primary.db.com -e "SELECT 1" > /dev/null 2>&1
  return $?
}

# 转移从变主
promote_replica() {
  mysql -h replica.db.com -e "STOP SLAVE;"
  mysql -h replica.db.com -e "RESET MASTER;"
  # 更新 DNS
  update_dns "db.com" "replica.db.com"
}

# 主循环
while true; do
  if ! check_primary; then
    echo "Primary is down, promoting replica..."
    promote_replica
    break
  fi
  sleep 10
done
```

### 特点

**优点**：
```
✓ 相对简单：只需要主从复制
✓ 成本低：只需要两台机器
✓ 零停机：故障转移只需 DNS 更新
✓ 数据安全：同步复制可以保证无数据丢失
```

**缺点**：
```
✗ 脑裂风险：如果监控和主实例都故障，会有问题
✗ DNS 缓存：客户端可能缓存旧 DNS，故障转移可能需要 5-10 分钟
✗ 备实例闲置：备实例平时只读，浪费资源
✗ 无法处理应用故障：只能处理主机故障，不能处理应用 bug
```

### 应用场景

```
适合：
  ├─ 简单的单体应用
  ├─ 对成本敏感
  ├─ 可以接受几分钟的转移时间
  └─ 流量相对稳定

不适合：
  ├─ 需要极短的 RTO（< 1 分钟）
  ├─ 需要处理应用故障
  ├─ 需要跨地域的高可用
  └─ 微服务架构
```

---

## 模式 2：多可用区部署（Multi-AZ Deployment）

### 架构

```
用户请求
  ↓
负载均衡器（跨多 AZ）
  ├─ 可用区 1A
  │  ├─ 应用实例 1
  │  ├─ 缓存实例 1
  │  └─ 数据库主库 1
  ├─ 可用区 1B
  │  ├─ 应用实例 2
  │  ├─ 缓存实例 2
  │  └─ 数据库副本 1
  └─ 可用区 1C（可选）
     └─ 数据库副本 2（只读）

网络：
  └─ 所有 AZ 通过内网相连
```

### 实现

**AWS 例子**：
```yaml
# CloudFormation 模板
AWSTemplateFormatVersion: '2010-09-09'

Resources:
  # 跨 AZ 的应用负载均衡
  ALB:
    Type: AWS::ElasticLoadBalancingV2::LoadBalancer
    Properties:
      Subnets:
        - subnet-1a
        - subnet-1b
        - subnet-1c

  # 自动扩展组（跨 AZ）
  ASG:
    Type: AWS::AutoScaling::AutoScalingGroup
    Properties:
      AvailabilityZones:
        - us-east-1a
        - us-east-1b
        - us-east-1c
      MinSize: 3
      MaxSize: 9
      DesiredCapacity: 3

  # 多 AZ RDS 数据库
  Database:
    Type: AWS::RDS::DBInstance
    Properties:
      MultiAZ: true
      DBSubnetGroupName: multi-az-group
      AvailabilityZone: us-east-1a
```

### 特点

**优点**：
```
✓ 高可用：可以处理单 AZ 故障
✓ 自动转移：故障自动检测和转移（通常 < 1 分钟）
✓ 资源利用：所有实例都在使用（不像主从模式浪费备实例）
✓ 性能分散：流量分散到多 AZ
✓ 地震或数据中心火灾：单 AZ 故障不影响服务
```

**缺点**：
```
✗ 成本增加：需要 2-3 倍的资源
✗ 网络延迟：跨 AZ 通信有额外延迟（< 10ms 通常可接受）
✗ 数据一致性：跨 AZ 同步可能有延迟
✗ 复杂性：需要管理多个 AZ 的配置
```

### 应用场景

```
适合：
  ├─ 中大型应用
  ├─ 需要 99.99% 可用性
  ├─ 有充足的预算
  ├─ 云上部署
  └─ 关键业务应用

不适合：
  ├─ 小型或创业项目
  ├─ 成本敏感的项目
  ├─ 单数据中心部署
  └─ 不需要高可用的应用
```

---

## 模式 3：活跃-活跃部署（Active-Active Deployment）

### 架构

```
用户请求
  ↓
全局负载均衡（智能路由）
  ├─ 区域 A
  │  ├─ 应用实例组
  │  ├─ 缓存实例组
  │  └─ 数据库实例（主）
  └─ 区域 B
     ├─ 应用实例组
     ├─ 缓存实例组
     └─ 数据库实例（主）

数据同步：
  ├─ 区域 A ↔ 区域 B 双向复制
  ├─ 冲突解决策略
  └─ 最终一致性
```

### 实现

**全局负载均衡**：
```python
# Route53（AWS 例子）
# 基于地理位置和健康检查的路由

from boto3 import client

route53 = client('route53')

# 创建健康检查
route53.create_health_check(
    Type='HTTPS',
    ResourcePath='/health',
    FullyQualifiedDomainName='region-a.example.com'
)

# 创建路由策略
route53.change_resource_record_sets(
    Changes=[
        {
            'Action': 'CREATE',
            'ResourceRecordSet': {
                'Name': 'app.example.com',
                'Type': 'A',
                'SetIdentifier': 'region-a',
                'GeolocationContinent': 'NA',
                'AliasTarget': {
                    'HostedZoneId': 'Z123',
                    'DNSName': 'region-a.example.com',
                    'EvaluateTargetHealth': True
                }
            }
        },
        {
            'Action': 'CREATE',
            'ResourceRecordSet': {
                'Name': 'app.example.com',
                'Type': 'A',
                'SetIdentifier': 'region-b',
                'GeolocationContinent': 'EU',
                'AliasTarget': {
                    'HostedZoneId': 'Z456',
                    'DNSName': 'region-b.example.com',
                    'EvaluateTargetHealth': True
                }
            }
        }
    ]
)
```

**数据同步（Cassandra 例子）**：
```yaml
# Cassandra 集群（多数据中心）
cassandra:
  cluster_name: MyApp
  listen_address: 10.0.0.1

  data_centers:
    - name: us-east
      replication_factor: 3
      seeds:
        - 10.0.1.1
        - 10.0.1.2

    - name: eu-west
      replication_factor: 3
      seeds:
        - 10.1.1.1
        - 10.1.1.2

# 复制策略
keyspace:
  replication: {
    'class': 'NetworkTopologyStrategy',
    'us-east': 3,
    'eu-west': 3
  }
```

### 特点

**优点**：
```
✓ 完全消除单点故障：任何单个数据中心故障都不影响
✓ 性能最好：可以就近路由用户到最近的数据中心
✓ 极高可用：99.999% 可用性
✓ 低延迟：用户连接到最近的数据中心
```

**缺点**：
```
✗ 成本最高：需要 2 倍的完整基础设施
✗ 复杂度最高：数据同步和冲突解决复杂
✗ 数据一致性困难：跨地域同步会有延迟
✗ 运维难度最高：需要专业的分布式系统知识
```

### 应用场景

```
适合：
  ├─ 全球化应用
  ├─ 对可用性有极高要求（99.999%+）
  ├─ 有充足的资源和团队
  ├─ 支付或金融等关键系统
  └─ 大型互联网公司

不适合：
  ├─ 大多数应用（成本和复杂性太高）
  ├─ 需要强一致性的应用
  ├─ 没有分布式系统专家的团队
  └─ 中小企业应用
```

---

## 灰度发布模式

### 模式 1：金丝雀发布（Canary Release）

**流程**：
```
第 1 阶段：1% 用户
  ├─ 部署新版本到 1% 的实例
  ├─ 监控错误率、延迟、异常
  ├─ 如果指标异常，回滚
  └─ 持续监控 30 分钟

第 2 阶段：5% 用户
  ├─ 逐步增加到 5%
  ├─ 观察是否有问题
  └─ 继续部署或回滚

第 3 阶段：25% 用户
  └─ 继续监控

第 4 阶段：50% 用户
  └─ 继续监控

第 5 阶段：100% 用户
  └─ 全量部署完成

总时间：通常 2-4 小时
```

**实现（Kubernetes）**：
```yaml
apiVersion: fluxcd.io/v1beta1
kind: Canary
metadata:
  name: myapp
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp

  # 金丝雀配置
  progressDeadlineSeconds: 300
  skipAnalysis: false

  # 分析配置
  analysis:
    interval: 1m
    threshold: 5
    metrics:
    - name: error_rate
      thresholdRange:
        max: 1
    - name: latency
      thresholdRange:
        max: 500

  # 部署阶段
  stages:
  - weight: 1
    duration: 1m
  - weight: 5
    duration: 2m
  - weight: 25
    duration: 3m
  - weight: 50
    duration: 5m
```

### 模式 2：蓝绿发布（Blue-Green Deployment）

**流程**：
```
初始状态：
  蓝（B1）：运行旧版本，处理所有流量
  绿（G1）：待命

部署新版本：
  步骤 1：在绿（G2）环境部署新版本
  步骤 2：测试绿（G2）
  步骤 3：验证绿（G2）的所有功能
  步骤 4：切换流量：蓝 → 绿

监控新版本：
  步骤 5：蓝（G1）保留 24 小时
  步骤 6：如有问题，快速切回蓝（G1）

清理：
  步骤 7：确认绿正常，关闭蓝

关键特性：
  ├─ 零停机时间
  ├─ 快速回滚（仅需切换）
  ├─ 完整的测试验证
  └─ 需要 2 倍的资源

总时间：30 分钟 - 2 小时
```

**实现**：
```bash
#!/bin/bash

# 蓝绿发布脚本

# 1. 部署绿色环境
echo "Deploying green environment..."
docker-compose -f docker-compose.green.yml up -d

# 2. 运行测试
echo "Running tests..."
./run-tests.sh green
if [ $? -ne 0 ]; then
  echo "Tests failed, rolling back..."
  docker-compose -f docker-compose.green.yml down
  exit 1
fi

# 3. 切换流量
echo "Switching traffic..."
# 更新负载均衡器指向绿
aws elb modify-load-balancer-attributes \
  --load-balancer-name myapp-lb \
  --load-balancer-attributes "{\"CrossZoneLoadBalancing\":{\"Enabled\":true}}" \
  --instances i-green1 i-green2 i-green3

# 4. 监控
echo "Monitoring..."
sleep 300  # 监控 5 分钟

# 5. 验证成功
if check_health green; then
  echo "Deployment successful!"
  # 关闭蓝环境
  docker-compose -f docker-compose.blue.yml down
else
  echo "Deployment failed, rolling back..."
  # 切换回蓝
  aws elb modify-load-balancer-attributes \
    --load-balancer-name myapp-lb \
    --instances i-blue1 i-blue2 i-blue3
  docker-compose -f docker-compose.green.yml down
fi
```

---

## 高可用架构选择框架

```
Question 1：需要的可用性？
  99.9% (6 个 9)      → 多 AZ + 自动转移
  99.99% (8 个 9)     → 多 AZ + 高可用中间件
  99.999% (10 个 9)   → 活跃-活跃 + 多地域

Question 2：关键指标（RTO / RPO）？
  RTO < 1 分钟 / RPO < 5 分钟    → 自动转移或多 AZ
  RTO < 5 分钟 / RPO < 30 分钟   → 多 AZ + 监控告警
  RTO < 1 小时 / RPO < 1 小时    → 定期备份 + 灾难恢复计划

Question 3：预算？
  低     → 单机 + 手动备份
  中     → 多 AZ 部署
  高     → 活跃-活跃 + 多地域

Question 4：团队能力？
  入门   → 多 AZ + 托管服务
  中级   → 多 AZ + 自动转移
  高级   → 活跃-活跃 + 自定义解决方案

建议架构：
  初期：多 AZ（性价比最好）
  中期：多 AZ + 自动转移 + 灾备计划
  长期：根据业务发展考虑活跃-活跃
```

---

## 总结：高可用架构的 4 个关键要素

```
1. 冗余（Redundancy）
   ├─ 多个实例（避免单点故障）
   ├─ 多个数据中心（避免地点单点故障）
   └─ 多个备份（避免数据丢失）

2. 检测（Detection）
   ├─ 主动监控（定期检查健康状态）
   ├─ 告警（异常立即通知）
   └─ 自动检测（无需人工干预）

3. 转移（Failover）
   ├─ 自动转移（无需人工介入）
   ├─ 快速转移（RTO 尽可能短）
   └─ 完整转移（应用和数据都转移）

4. 恢复（Recovery）
   ├─ 快速恢复（失败的实例快速恢复）
   ├─ 数据恢复（从备份恢复数据）
   └─ 状态同步（恢复的实例与集群同步）

所有这 4 个要素都必须到位，才能建立真正的高可用系统。
```

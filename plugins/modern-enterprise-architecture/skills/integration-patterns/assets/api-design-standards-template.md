# API 设计标准化模板

本模板帮助您在应用间集成时定义清晰、一致的 API 规范。

## Part 1：API 基础信息

```
API 名称：_________________________________
版本：v1.0
发布日期：____-____-____
维护团队：________________________________
联系方式：________________________________
文档地址：________________________________
```

## Part 2：API 功能定义

### API 1：创建资源

```
操作：创建订单

HTTP 方法：POST
URL：/api/v1/orders

请求头：
  Content-Type: application/json
  Authorization: Bearer {token}
  X-Request-ID: {unique_id}  # 用于链路追踪

请求体：
```json
{
  "customerId": "string (必填，顾客 ID)",
  "items": "array (必填，订单项)",
  "items[0]": {
    "productId": "string (必填，产品 ID)",
    "quantity": "integer (必填，数量)",
    "unitPrice": "decimal (必填，单价)"
  },
  "shippingAddress": "string (必填，地址)",
  "paymentMethod": "string (必填，支付方式)",
  "couponCode": "string (可选，优惠券)"
}
```

响应（201 Created）：
```json
{
  "orderId": "ORD123456",
  "status": "CREATED",
  "totalAmount": 1000.00,
  "createdAt": "2024-01-15T10:30:00Z"
}
```

错误响应（400）：
```json
{
  "error": {
    "code": "INVALID_INPUT",
    "message": "顾客 ID 不存在",
    "details": {
      "field": "customerId",
      "value": "invalid"
    }
  }
}
```

SLA：
  ├─ 响应时间：P95 < 200ms
  ├─ 可用性：99.9%
  ├─ 并发：> 10k req/s

重试策略：
  ├─ 可幂等（POST 请求加入幂等 key）
  ├─ 重试次数：3 次
  └─ 退避策略：1s, 2s, 4s

版本管理：
  当前版本：v1
  过期版本：v0（不再支持）
  将来版本：v2（计划 2024-06-01）
  兼容性：v1 和 v2 并存 6 个月
```

### API 2：查询资源（其他 API 类似定义）

```
操作：查询订单
HTTP 方法：GET
URL：/api/v1/orders/{orderId}
...
```

### API 3：更新资源

```
操作：更新订单
HTTP 方法：PUT / PATCH
URL：/api/v1/orders/{orderId}
...
```

### API 4：删除资源

```
操作：取消订单
HTTP 方法：DELETE
URL：/api/v1/orders/{orderId}
...
```

## Part 3：错误处理规范

```
HTTP Status Code 规范：

2xx 成功
  200：OK（GET 成功）
  201：Created（POST 成功创建）
  204：No Content（DELETE 成功）

4xx 客户端错误
  400：Bad Request（输入无效）
  401：Unauthorized（未认证）
  403：Forbidden（无权限）
  404：Not Found（资源不存在）
  409：Conflict（数据冲突，如 ID 重复）
  429：Too Many Requests（限流）

5xx 服务器错误
  500：Internal Server Error（未知错误）
  502：Bad Gateway（依赖服务故障）
  503：Service Unavailable（服务故障）
  504：Gateway Timeout（依赖服务超时）

错误响应格式（统一）：
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "用户友好的错误信息",
    "details": {
      "field": "字段名（如果适用）",
      "value": "错误的值"
    },
    "requestId": "X-Request-ID 值（用于追踪）"
  }
}
```

常见错误码：
  INVALID_INPUT：输入验证失败
  RESOURCE_NOT_FOUND：资源不存在
  UNAUTHORIZED：认证失败
  FORBIDDEN：授权失败
  CONFLICT：数据冲突
  RATE_LIMIT_EXCEEDED：限流
  INTERNAL_ERROR：内部错误
  SERVICE_UNAVAILABLE：服务不可用
  TIMEOUT：请求超时
```

## Part 4：安全规范

```
□ 身份验证（Authentication）
  方式：JWT Bearer Token
  获取方式：调用 /auth/token
  Token 格式：Bearer {token}
  Token 有效期：1 小时
  刷新 Token：需要调用 /auth/refresh

□ 授权（Authorization）
  权限级别：
    - admin：管理员，可以访问所有 API
    - user：普通用户，只能访问自己的资源
    - guest：游客，只读访问

□ 请求签名
  对于敏感操作，需要签名
  签名方式：HMAC-SHA256
  签名内容：method + url + body + timestamp
  签名头：X-Signature

□ 速率限制
  限制规则：每个用户 1000 req/小时
  超过限制：返回 429 Too Many Requests
  限制信息在响应头中：
    X-RateLimit-Limit: 1000
    X-RateLimit-Remaining: 999
    X-RateLimit-Reset: 1234567890
```

## Part 5：分布式追踪

```
所有请求都必须支持分布式追踪

请求头：
  X-Request-ID：全局唯一的请求 ID
  X-Trace-ID：链路追踪 ID
  X-Span-ID：当前服务的 span ID
  X-Parent-Span-ID：父服务的 span ID

调用链：
  Client → API Gateway → Service A → Service B
    ├─ Request-ID：REQ-123（全局唯一）
    ├─ Trace-ID：TRACE-456（链路追踪）
    ├─ A 的 Span-ID：SPAN-A-1
    └─ B 的 Parent-Span-ID：SPAN-A-1

日志记录：
  每个 API 调用都必须记录：
    ├─ 时间戳
    ├─ Request-ID
    ├─ 客户端 IP
    ├─ 方法、URL、状态码
    ├─ 响应时间
    ├─ 用户身份
    └─ 任何错误信息

日志级别：
  DEBUG：开发调试
  INFO：正常请求（默认）
  WARN：警告（如 API 超时）
  ERROR：错误（如调用失败）
```

## Part 6：API 文档

```
必须提供 OpenAPI 3.0 规范：

openapi: 3.0.0
info:
  title: Order API
  version: 1.0.0
servers:
  - url: https://api.example.com
paths:
  /orders:
    post:
      summary: 创建订单
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateOrderRequest'
      responses:
        '201':
          description: 订单已创建
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OrderResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
  /orders/{orderId}:
    get:
      summary: 查询订单
      parameters:
        - name: orderId
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: 订单信息
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OrderResponse'

components:
  schemas:
    CreateOrderRequest:
      type: object
      required:
        - customerId
        - items
        - shippingAddress
      properties:
        customerId:
          type: string
        items:
          type: array
          items:
            $ref: '#/components/schemas/OrderItem'
    OrderItem:
      type: object
      required:
        - productId
        - quantity
      properties:
        productId:
          type: string
        quantity:
          type: integer
    OrderResponse:
      type: object
      properties:
        orderId:
          type: string
        status:
          type: string
          enum: [CREATED, CONFIRMED, COMPLETED, CANCELLED]
        createdAt:
          type: string
          format: date-time

  responses:
    BadRequest:
      description: 请求无效
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ErrorResponse'
    NotFound:
      description: 资源不存在
    Unauthorized:
      description: 未认证
    Forbidden:
      description: 无权限

security:
  - bearerAuth: []

securitySchemes:
  bearerAuth:
    type: http
    scheme: bearer
    bearerFormat: JWT
```

## Part 7：API 版本管理

```
版本管理策略：

URL 路径版本：
  /api/v1/orders  （v1 版本）
  /api/v2/orders  （v2 版本）
  优点：清晰，便于 URL 路由
  缺点：URL 冗长

Header 版本：
  Accept-Version: 1.0
  优点：URL 简洁
  缺点：不太直观

版本生命周期：
  新版本发布 → 旧版本并存 6 个月 → 旧版本弃用 → 旧版本下线

  示例：
    2024-01-01：发布 v2（v1 仍支持）
    2024-07-01：v1 弃用，仅 v2 支持
    2024-12-31：v1 下线

向后兼容性承诺：
  □ 同一主版本（v1.x）内保证兼容
  □ 新增字段不影响旧客户端
  □ 移除字段需要至少 6 个月的过期期
  □ 修改现有字段的含义需要新版本
```

## Part 8：API 监控告警

```
关键指标：

1. API 可用性
   告警条件：< 99.9%
   检查频率：每 5 分钟

2. API 响应时间
   告警条件：P95 > 300ms
   检查频率：每 1 分钟

3. API 错误率
   告警条件：> 1%
   检查频率：每 1 分钟

4. 限流触发
   告警条件：> 100 次/小时
   检查频率：每 15 分钟

5. 依赖服务故障
   告警条件：调用失败 > 5%
   检查频率：每 1 分钟

告警接收人：API 所有者、值班人
告警升级：15 分钟未响应 → 升级到技术主管
```

---

## 总结

一份清晰的 API 规范应该包括：
```
✅ 清晰的功能定义（操作、参数、响应）
✅ 统一的错误处理
✅ 严格的安全规范
✅ 完整的文档（OpenAPI）
✅ 版本管理策略
✅ 监控和告警规则

这样才能保证：
  - 调用者能快速理解和使用
  - 应用能稳定可靠地集成
  - 问题能快速定位和解决
  - 演进能平滑进行
```


# Server 开发指南

## 消息撤回

撤回用户发送的消息。

方法名：`/message/recall`

签名方法：请参考 [通用 API 接口签名规则](server.html#通用_API_接口签名规则)

URL：[http://你自已的域名或 IP + 端口/message/recall.\[format\]](#)

[format] 表示返回格式，可以为 `json` 或 `xml`，注意不要带 [ ]。

HTTP 方法：`POST`

** 表单参数 **

 名称     | 类型   | 说明
:---------|:-------|:------------------
 `fromUserId` | String | 发送人用户 Id。（必传）
 `conversationType` | Int | 会话类型，二人会话是 1 、讨论组会话是 2 、群组会话是 3 。（必传）
 `targetId` | String | 目标 Id，根据不同的 ConversationType，可能是用户 Id、讨论组 Id、群组 Id。（必传）
 `messageUID` | String | 消息 Id。（必传）
 `sentTime` | Long | 消息发送时间。（必传）

** 返回值 **

 名称   | 类型 | 说明
:-------|:-----|:-------------------
 `code` | Int  | 返回码，200 为正常。

`JSON 格式：`

```
{"code":200}
```

`XML 格式：`

```
<xml>
  <code>200</code>
</xml>
```

返回值请参考 [API 方法返回值说明](server.html#API_方法返回值说明)

#### 示例

HTTP 请求:

```
POST /message/recall.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
App-Key: uwd1c0sxdlx2
Nonce: 14314
Timestamp: 1408706337
Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
Content-Type: application/x-www-form-urlencoded

fromUserId=fDR2cVpxxR5zSMUNh3yAwh&targetId=MersNRhaKwJkRV9mJR5JXY&conversationType=1&messageUID=5FGT-7VA9-G4DD-4V5P&sentTime=1507778882124
```

HTTP 响应:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{"code":200}
```

## API 方法返回值说明

### HTTP 状态码

code | 描述             | 详细解释
:-----|:-----------------|:---------------------------------------------
200  | 成功             | 成功
400  | 错误请求         | 该请求是无效的，详细的错误信息会说明原因
401  | 未授权           | 验证失败，详细的错误信息会说明原因
403  | 服务器拒绝请求    | 被拒绝调用，详细的错误信息会说明原因
404  | 未找到          | 服务器找不到请求的地址
405  | 方法禁用          | 群容量超出上限，禁止调用
429  | 太多的请求        | 超出了调用频率限制，详细的错误信息会说明原因
500  | 服务器内部错误      | 服务器内部出错了，请联系我们尽快解决问题
504  | 网关超时          | 服务器在运行，本次请求响应超时，请稍后重试

### 业务返回码

 code | 描述             | 详细解释                               | HTTP 状态码
:-----|:-----------------|:---------------------------------------|:------------
 404  | 未找到           | 服务器找不到请求的地址               | 404
 1000 | 服务内部错误     | 服务器端内部逻辑错误,请稍后重试        | 500
 1001 | App Secret 错误  | App Key 与 App Secret 不匹配           | 401
 1002 | 参数错误         | 参数错误，详细的描述信息会说明         | 400
 1003 | 无 POST 数据     | 没有 POST 任何数据                     | 400
 1004 | 验证签名错误     | 验证签名错误                           | 401
 1005 | 参数长度超限     | 参数长度超限，详细的描述信息会说明      | 400
 1006 | App 被锁定或删除 | App 被锁定或删除                       | 401
 1007 | 被限制调用       | 该方法被限制调用，详细的描述信息会说明  | 401
 1008 | 调用频率超限     | 调用频率超限，详细的描述信息会说明，广播消息未开通时也会返回此状态码。 | 429
 1009 | 服务未开通      | 未开通该服务，请到开发者管理后台开通或提交工单申请。    | 430
 1015 | 删除的数据不存在  | 要删除的保活聊天室 ID 不存在。            | 200
 1016 | 设置保活聊天室个数超限 | 设置的保活聊天室个数超限。            | 403
 1050 | 内部服务超时     | 内部服务响应超时                       | 504
 2007 | 测试用户数量超限 | 测试用户数量超限                       | 403
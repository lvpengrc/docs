---
title: Server 开发指南 - 融云 RongCloud
description: 提供融云服务器端 API 开发的相关帮助和指南。
template: doc.jade
---
# Server 开发指南

## 高保障服务端消息路由

高保障服务端消息路由，是基于 kafka 消息中间件的方式实现的，在高并发情况下可保障消息不丢失，服务端存储 3 小时以内的消息，开发者需要在 3 个小时内取走消息，3 个小时后的消息将不在存储。

<div class="bs-callout bs-callout-warning">
高保障服务端消息路由功能，需开启后才能使用。
</div>

### 获取单聊消息

方法名：`/rtmsg/person/qrymsg`

调用频率：每秒钟最多请求 10 次，超出的请求返回空数据。

签名方法：请参考 [通用 API 接口签名规则](server.html#通用_API_接口签名规则)

URL：[http://api.cn.ronghub.com/rtmsg/person/qrymsg.json

HTTP 方法：`POST`

** 表单参数 **

无

#### 示例

HTTP 请求:

```
POST /rtmsg/person/qrymsg.json HTTP/1.1
Host: api.cn.ronghub.com
App-Key: uwd1c0sxdlx2
Nonce: 14314
Timestamp: 1408706337
Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/x-www-form-urlencoded
```

** 返回值 **

 名称         | 类型   | 说明
:-------------|:-------|:---------------------------------------
 `results`    | String[] | 消息数据
 `fromUserId` | String | 发送用户 Id。
 `toUserId`   | String | 接收用户 Id，即为客户端 `targetId`。
 `objectName` | String | 消息类型，文本消息 `RC:TxtMsg` 、 图片消息 `RC:ImgMsg` 、语音消息 `RC:VcMsg` 、图文消息 `RC:ImgTextMsg` 、位置消息 `RC:LBSMsg` 、添加联系人消息 `RC:ContactNtf` 、提示条通知消息 `RC:InfoNtf` 、资料通知消息 `RC:ProfileNtf` 、通用命令通知消息 `RC:CmdNtf` 、客服握手消息 `RC:HsMsg` 、客服挂断消息 `RC:SpMsg`，详细请参见[消息类型说明文档](android_architecture.html#消息的分类)。
 `content`    | String | 发送消息内容。
 `channelType`   | String    | 会话类型，二人会话是 `PERSON` 、讨论组会话是 `PERSONS` 、群组会话是 `GROUP` 、聊天室会话是 `TEMPGROUP` 、客服会话是 `CUSTOMERSERVICE` 、 系统通知是 `NOTIFY` 、应用公众服务是 `MC` 、公众服务是 `MP`。<br>客户端 SDK 中为 `ConversationType` ，二人会话是 `1` 、讨论组会话是 `2` 、群组会话是 `3` 、聊天室会话是 `4` 、客服会话是 `5` 、 系统通知是 `6` 、应用公众服务是 `7` 、公众服务是 `8`。
 `msgTimestamp`  | String | 服务端收到客户端发送消息时的服务器时间（1970年到现在的毫秒数）。
 `msgUID`  | String | 可通过 `msgUID` 确定消息唯一。
 `sensitiveType` | Int | 消息中是否含有敏感词标识，0 为不含有敏感词，1 为含有屏蔽敏感词，2 为含有替换敏感词。消息路由功能默认含有屏蔽敏感词的消息不进行路由，可申请开通含有敏感词的消息路由功能，未开通情况下 sensitiveType 值默认为 0 不代表任何意义。开通后可通过该属性判断消息中是否含有敏感词。目前支持单聊、群聊、聊天室会话类型，其他会话类型默认为 0 ，开通后含有屏蔽敏感词的消息也不会进行下发，只会进行消息路由。
 `source`  | Int  | 标识消息的发送源头，包括：iOS、Android、Websocket。目前支持单聊、群聊会话类型，其他会话类型为空。
 `size`  | Int  | 本次请求获取的消息条数。

 `JSON 格式：`

```
{
   "results": [
       {
           "fromUserId": "877216513634336768",
           "toUserId": "881916345615646720",
           "msgTimestamp": "1499141156189",
           "timestamp": "1499141156189",
           "objectName": "app:lettermsgcontent",
           "content": {
               "content": "[Video]",
               "miniversion": 1,
               "readState": 1
           },
           "msgUID": "5EGN-HLDB-K4Q4-LQ1G",
           "channelType": "PERSON"
       }
   ],
   "size": 1
}
```

### 获取群组消息

方法名：`/rtmsg/group/qrymsg`

调用频率：每秒钟最多请求 10 次，超出的请求返回空数据。

签名方法：请参考 [通用 API 接口签名规则](server.html#通用_API_接口签名规则)

URL：[http://api.cn.ronghub.com/rtmsg/group/qrymsg.json

HTTP 方法：`POST`

** 表单参数 **

无

#### 示例

HTTP 请求:

```
POST /rtmsg/group/qrymsg.json HTTP/1.1
Host: api.cn.ronghub.com
App-Key: uwd1c0sxdlx2
Nonce: 14314
Timestamp: 1408706337
Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/x-www-form-urlencoded
```

** 返回值 **

 名称         | 类型   | 说明
:-------------|:-------|:---------------------------------------
 `results`    | String[] | 消息数据
 `fromUserId` | String | 发送用户 Id。
 `toUserId`   | String | 接收用户 Id，即为客户端 `targetId`。
 `objectName` | String | 消息类型，文本消息 `RC:TxtMsg` 、 图片消息 `RC:ImgMsg` 、语音消息 `RC:VcMsg` 、图文消息 `RC:ImgTextMsg` 、位置消息 `RC:LBSMsg` 、添加联系人消息 `RC:ContactNtf` 、提示条通知消息 `RC:InfoNtf` 、资料通知消息 `RC:ProfileNtf` 、通用命令通知消息 `RC:CmdNtf` 、客服握手消息 `RC:HsMsg` 、客服挂断消息 `RC:SpMsg`，详细请参见[消息类型说明文档](android_architecture.html#消息的分类)。
 `content`    | String | 发送消息内容。
 `channelType`   | String    | 会话类型，二人会话是 `PERSON` 、讨论组会话是 `PERSONS` 、群组会话是 `GROUP` 、聊天室会话是 `TEMPGROUP` 、客服会话是 `CUSTOMERSERVICE` 、 系统通知是 `NOTIFY` 、应用公众服务是 `MC` 、公众服务是 `MP`。<br>客户端 SDK 中为 `ConversationType` ，二人会话是 `1` 、讨论组会话是 `2` 、群组会话是 `3` 、聊天室会话是 `4` 、客服会话是 `5` 、 系统通知是 `6` 、应用公众服务是 `7` 、公众服务是 `8`。
 `msgTimestamp`  | String | 服务端收到客户端发送消息时的服务器时间（1970年到现在的毫秒数）。
 `msgUID`  | String | 可通过 `msgUID` 确定消息唯一。
 `sensitiveType` | Int | 消息中是否含有敏感词标识，0 为不含有敏感词，1 为含有屏蔽敏感词，2 为含有替换敏感词。消息路由功能默认含有屏蔽敏感词的消息不进行路由，可申请开通含有敏感词的消息路由功能，未开通情况下 sensitiveType 值默认为 0 不代表任何意义。开通后可通过该属性判断消息中是否含有敏感词。目前支持单聊、群聊、聊天室会话类型，其他会话类型默认为 0 ，开通后含有屏蔽敏感词的消息也不会进行下发，只会进行消息路由。
 `source`  | Int  | 标识消息的发送源头，包括：iOS、Android、Websocket。目前支持单聊、群聊会话类型，其他会话类型为空。
 `size`  | Int  | 本次请求获取的消息条数。

 `JSON 格式：`

```
{
   "results": [
       {
           "fromUserId": "877216513634336768",
           "toUserId": "881916345615646720",
           "msgTimestamp": "1499141156189",
           "timestamp": "1499141156189",
           "objectName": "app:lettermsgcontent",
           "content": {
               "content": "[Video]",
               "miniversion": 1,
               "readState": 1
           },
           "msgUID": "5EGN-HLDB-K4Q4-LQ1G",
           "channelType": "PERSON"
       }
   ],
   "size": 1
}
```

### 获取聊天室消息

目前，kafka 消息路由服务中，聊天室会话只支持加入、退出聊天室的事件路由功能。

方法名：`/rtmsg/chatroom/qrymsg`

调用频率：每秒钟最多请求 10 次，超出的请求返回空数据。

签名方法：请参考 [通用 API 接口签名规则](server.html#通用_API_接口签名规则)

URL：[http://api.cn.ronghub.com/rtmsg/chatroom/qrymsg.json

HTTP 方法：`POST`

** 表单参数 **

无

#### 示例

HTTP 请求:

```
POST /rtmsg/chatroom/qrymsg.json HTTP/1.1
Host: api.cn.ronghub.com
App-Key: uwd1c0sxdlx2
Nonce: 14314
Timestamp: 1408706337
Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/x-www-form-urlencoded
```

** 返回值 **

名称         | 类型   | 说明
:-------------|:-------|:---------------------------------------
`results`    | String[] | 消息数据
`fromUserId` | String | 发送用户 Id。
`toUserId`   | String | 接收用户 Id，即为客户端 `targetId`。
`msgTimestamp`  | String | 服务端收到客户端发送消息时的服务器时间（1970年到现在的毫秒数）。
`objectName` | String | 消息类型，加入聊天室事件 `func:joinchatroommsgcontent` 、 退出聊天室事件 `func:leavechatroommsgcontent`
`size`  | Int  | 本次请求获取的消息条数。

`JSON 格式：`

```
{
  "results": [
      {
        "fromUserId": "22810522",
        "toUserId": "10889854",
        "msgTimestamp": "1499097988346",
        "objectName": "func:leavechatroommsgcontent"
      }
  ],
  "size": 1
}
```

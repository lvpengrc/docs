---
title: 融云应用公众服务开发者文档 - 融云 RongCloud
description: 提供融云应用公众服务 API 开发的相关帮助和指南。
template: doc.jade
---
# 应用公众服务开发文档

<br>

## 应用公众号设置

### 创建应用公众号

接入融云应用公众服务需要通过以下方法创建应用公众号。

** 示例 **

Request:

```
POST /mc/add.json HTTP/1.1
Host:你自已的域名或 IP + 端口
Content-Type: Application/x-www-form-urlencoded

mcid=testmcid1&displayName=第二个应用内公众号&portraitUrl=http%3A%2F%2Fwww.rongcloud.cn%2Fimages%2Flogo.png&uiType=0&introduction=测试应用内公众号说明描述&RC-App-Key=x18ywvq83onzm&isFollow=true&routeUrl=http%3A%2F%2Fwww.rongcloud.cn%2Fimages%2F
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:----------------------------------------------------------------------------------------
 `mcid`      | 是       | 公众号 ID ，同一 App Key 下 mcid 值不能重复创建多次。
 `displayName` | 是       | 公众号名称。
 `portraitUrl` | 是       | 公众号头像地址。
 `uiType`   | 是       | 显示类型 0 在会话列表中显示。
 `introduction`   | 是       | 公众号描述信息。
 `RC-App-Key`   | 是       | 分配的 App Key。
 `isFollow`   | 是       | 是否为全局应用公众号，true 表示为是，false 表示为否。
 `routeUrl`   | 是       | 接收客户端发送该公众号消息的接收地址。

Response:

正常示例：

```
{"info":{"id":0,"mcid":"testmcid0","mcidHash":0,"fcid":0,"DisplayName":"刷新应用内公众号","IsFollow":true,"PortraitUrl":"http://www.rongcloud.cn/images/logo.png","RouteUrl":"http://www.rongcloud.cn","FuncRouteUrl":"","uiType":0,"isRouteOpen":true,"IsAuthorization":false,"IsGetUserInfo":true,"Introduction":"测试应用内公众号说明描述","isCustom":false}}
```

错误示例：

```
{"errcode":1002,"errmsg":"错误描述"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 修改应用公众号信息

** 示例 **

Request:

```
POST /mc/refresh.json HTTP/1.1
Host:你自已的域名或 IP + 端口
Content-Type: Application/x-www-form-urlencoded

mcid=testmcid0&displayName=刷新应用内公众号&portraitUrl=http%3A%2F%2Fwww.rongcloud.cn%2Fimages%2Flogo.png&uiType=0&introduction=测试应用内公众号说明描述&RC-App-Key=x18ywvq83onzm
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:----------------------------------------------------------------------------------------
 `mcid`      | 是       | 公众号 ID 。
 `displayName` | 是       | 公众号名称。
 `portraitUrl` | 是       | 公众号头像地址。
 `uiType`   | 是       | 显示类型 0 在会话列表中显示。
 `introduction`   | 是       | 公众号描述信息。
 `RC-App-Key`   | 是       | 分配的 App Key。

Response:

正常示例：

```
{"info":{"id":0,"mcid":"testmcid0","mcidHash":0,"fcid":0,"DisplayName":"刷新应用内公众号","IsFollow":true,"PortraitUrl":"http://www.rongcloud.cn/images/logo.png","RouteUrl":"http://www.rongcloud.cn","FuncRouteUrl":"","uiType":0,"isRouteOpen":true,"IsAuthorization":false,"IsGetUserInfo":true,"Introduction":"测试应用内公众号说明描述","isCustom":false}}
```

错误示例：

```
{"errcode":1002,"errmsg":"错误描述"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 查询应用公众号信息

** 示例 **

Request:

```
POST /mc/info.json HTTP/1.1
Host:你自已的域名或 IP + 端口
Content-Type: Application/x-www-form-urlencoded

mcid=testmcid0&RC-App-Key=x18ywvq83onzm
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:----------------------------------------------------------------------------------------
 `mcid`      | 是       | 公众号 ID 。
 `RC-App-Key`   | 是       | 分配的 App Key。

Response:

正常示例：

```
{"info":{"id":0,"mcid":"testmcid0","mcidHash":0,"fcid":0,"DisplayName":"刷新应用内公众号","IsFollow":true,"PortraitUrl":"http://www.rongcloud.cn/images/logo.png","RouteUrl":"http://www.rongcloud.cn","FuncRouteUrl":"","uiType":0,"isRouteOpen":true,"IsAuthorization":false,"IsGetUserInfo":true,"Introduction":"测试应用内公众号说明描述","isCustom":false}}
```

错误示例：

```
{"errcode":1002,"errmsg":"错误描述"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 设置应用公众号路由地址

** 示例 **

Request:

```
POST /mc/route/setUrl.json HTTP/1.1
Host:你自已的域名或 IP + 端口
Content-Type: Application/x-www-form-urlencoded

mcid=testmcid0&RC-App-Key=x18ywvq83onzm&routeUrl=http%3A%2F%2Fwww.rongcloud.cn
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:----------------------------------------------------------------------------------------
 `mcid`      | 是       | 公众号 ID 。
 `RC-App-Key` | 是       | 分配的 App Key。
 `routeUrl`   | 是       | 接收客户端发送该公众号消息的接收地址。

Response:

正常示例：

```
{"info":{"id":0,"mcid":"testmcid0","mcidHash":0,"fcid":0,"DisplayName":"刷新应用内公众号","IsFollow":true,"PortraitUrl":"http://www.rongcloud.cn/images/logo.png","RouteUrl":"http://www.rongcloud.cn","FuncRouteUrl":"","uiType":0,"isRouteOpen":true,"IsAuthorization":false,"IsGetUserInfo":true,"Introduction":"测试应用内公众号说明描述","isCustom":false}}
```

错误示例：

```
{"errcode":1002,"errmsg":"错误描述"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 设置应用公众号路由开关

** 示例 **

Request:

```
POST /mc/route/setSwitch.json HTTP/1.1
Host:你自已的域名或 IP + 端口
Content-Type: Application/x-www-form-urlencoded

mcid=testmcid0&RC-App-Key=x18ywvq83onzm&isOpen=true
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:----------------------------------------------------------------------------------------
 `mcid`      | 是       | 公众号 ID 。
 `RC-App-Key` | 是       | 分配的 App Key。
 `isOpen`    | 是       | 消息路由功能状态，true 表示为开启，false 表示为关闭。

Response:

正常示例：

```
{"info":{"id":0,"mcid":"testmcid0","mcidHash":0,"fcid":0,"DisplayName":"刷新应用内公众号","IsFollow":true,"PortraitUrl":"http://www.rongcloud.cn/images/logo.png","RouteUrl":"http://www.rongcloud.cn","FuncRouteUrl":"","uiType":0,"isRouteOpen":true,"IsAuthorization":false,"IsGetUserInfo":true,"Introduction":"测试应用内公众号说明描述","isCustom":false}}
```

错误示例：

```
{"errcode":1002,"errmsg":"错误描述"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

## API 接口签名规则

### API 调用签名规则

本文档中所有请求融云服务端 API 接口的请求均使用此规则校验，以下不再重复说明。

每次请求 API 接口时，均需要提供 4 个 HTTP Request Header，具体如下：

 名称                          | 类型   | 说明
:------------------------------|:-------|:--------------------------------------------------------------
 `RC-App-Key`     | String | 管理后台分配的 App Key。  
 `RC-Nonce`       | String | 随机数，无长度限制。
 `RC-Timestamp`   | String | 时间戳，从 1970 年 1 月 1 日 0 点 0 分 0 秒开始到当前时间（北京时间）的毫秒数。（请严格参照此执行，服务器端会校验此信息）
 `RC-Signature`   | String | 数据签名。

 &emsp;&emsp;RC-Signature (数据签名)计算方法：将系统分配的 App Secret、RC-Nonce (随机数)、RC-Timestamp (时间戳)三个字符串按先后顺序拼接成一个字符串并进行 SHA1 哈希计算。如果调用的数据签名验证失败，接口调用会返回 HTTP 状态码 `401`。其他状态码请参见[状态码表](##_HTTP_状态码)。

### 签名生成代码示例

PHP 语言的代码示例：

```
// 重置随机数种子。
srand((double)microtime()*1000000);

$appSecret = 'Y1W2MeFwwwRxa0'; // 开发者平台分配的 App Secret。
$nonce = rand(); // 获取随机数。
$timestamp = time(); // 获取时间戳。

$signature = sha1($appSecret.$nonce.$timestamp);
```

HTTP 请求示例：

```
POST /user/setSwitch.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/x-www-form-urlencoded
Content-Length: 78

mcid=testmcid0&RC-App-Key=x18ywvq83onzm&isOpen=true
```
### API 接收签名规则

融云服务器向应用服务器推送数据（调用应用服务器接口）时会添加 3 个 GET 请求参数（在 URL 上添加的参数），具体如下：

 名称        | 类型   | 说明
:------------|:-------|:--------------------------------------------------
 `rc-nonce`     | String | 随机数，无长度限制。
 `rc-timestamp` | String | 时间戳，从1970年1月1日0点0分0秒开始到现在的秒数。
 `rc-signature` | String | 数据签名。

RC-Signature (数据签名)计算方法：将系统分配的 App Secret、RC-Nonce (随机数)、RC-Timestamp (时间戳)三个字符串按先后顺序拼接成一个字符串并进行 SHA1 哈希计算。

### 签名校验代码示例

PHP 语言的代码示例：

```
$appSecret = 'Y1W2MeFwwwRxa0'; // 开发者平台分配的 App Secret。
$nonce = $_GET['rc-nonce']; // 获取随机数。
$timestamp = $_GET['rc-timestamp']; // 获取时间戳。
$signature = $_GET['rc-signature']; // 获取数据签名。
$local_signature = sha1($appSecret.$nonce.$timestamp); // 生成本地签名。
if(strcmp($signature, $local_signature)===0){
	// TODO: 此处添加业务逻辑。
	echo 'OK';
} else {
	echo 'Error';
}
```

## 公众号菜单设置

### 添加公众号菜单

为公众号添加操作菜单，并设置菜单的响应事件。

方法名：/mc/menu/refresh.json

调用方式：参见 API 调用签名规则

URL：https://你自已的域名或 IP + 端口/mc/menu/refresh.json

** 添加公众号一级菜单示例 **

Request:

```
POST /mc/menu/refresh.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: Application/x-www-form-urlencoded

type=1&name=融云&url=http://www.xxx.com&level=1&mcid=whtest
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:---------------------------------------------------
 `mcid`      | 是       | 公众号 ID。
 `level`     | 是       | 菜单级别，支持 1、2 两级菜单，添加一级菜单时，此处为 1。
 `name`      | 是       | 菜单名称，最多不超过 10 个字符。
 `type`      | 是       | 菜单操作事件，0 为没有操作，1 为跳转到网页地址。
 `url`       | 否       | 跳转的网页地址，type 为 1 时有效。

Response:

正常示例：

```
{"info":{"id":0,"menuId":"07173179054d491fbe4eb2b40635253c","Type":1,"Name":"融云","Url":"http://www.xxx.com","Level":1,"Pid":"","mcid":"whtest","UpdateTime":null,"CreateTime":null,"order":0}}
```

返回值：

名称         | 类型 | 说明
:-------------|:---------|:---------------------------------------------------
`id`      | Int         | 序号 ID。
`menuId`     | String  | 菜单 ID。
`Type`      | Int       | 菜单操作事件，0 为没有操作，1 为跳转到网页地址。
`Name`      | String    | 菜单名称。
`Url`       | String    | 跳转的网页地址。
`Level`     | Int       | 菜单级别。
`Pid`      | String     | 所属父级菜单 ID，如当前菜单为 1 级，则 Pid 为空。
`mcid`      | String     | 公众号 ID。
`UpdateTime`| String    | 更新时间。
`CreateTime`| String    | 创建时间。
`order`     | Int       | 排序。

** 添加公众号二级菜单示例 **

Request:

```
POST /mc/menu/refresh.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: Application/x-www-form-urlencoded

type=0&name=测试1&url=http://www.baidu.com&level=2&mcid=whtest&pid=432531b2155040c0a425bd5962f7ec67
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:---------------------------------------------------
 `mcid`      | 是       | 公众号 ID。
 `level`     | 是       | 菜单级别，支持 1、2 两级菜单，添加二级菜单时，此处为 2。
 `name`      | 是       | 菜单名称，最多不超过 10 个字符。
 `pid`       | 是       | 所属一级菜单 ID。
 `type`      | 是       | 菜单操作事件，0 为没有操作，1 为跳转到网页地址。
 `url`       | 否       | 跳转的网页地址，type 为 1 时有效。

Response:

正常示例：

```
{"info":{"id":0,"menuId":"e4ae94df439d4693ae47a68be58ff6d8","Type":1,"Name":"测试","Url":"http://www.xxx.com","Level":2,"Pid":"432531b2155040c0a425bd5962f7ec67","mcid":"whtest","UpdateTime":null,"CreateTime":null,"order":0}}
```

返回值：

名称         | 类型 | 说明
:-------------|:---------|:---------------------------------------------------
`id`      | Int         | 序号 ID。
`menuId`     | String  | 菜单 ID。
`Type`      | Int       | 菜单操作事件，0 为没有操作，1 为跳转到网页地址。
`Name`      | String    | 菜单名称。
`Url`       | String    | 跳转的网页地址。
`Level`     | Int       | 菜单级别。
`Pid`      | String     | 所属父级菜单 ID。
`mcid`      | String     | 公众号 ID。
`UpdateTime`| String    | 更新时间。
`CreateTime`| String    | 创建时间。
`order`     | Int       | 排序。

### 删除公众号菜单

删除已添加的公众号菜单。

方法名：/mc/menu/delete.json

调用方式：参见 API 调用签名规则

URL：https://你自已的域名或 IP + 端口/mc/menu/delete.json

** 示例 **

Request:

```
POST /mc/menu/delete.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: Application/x-www-form-urlencoded

mcid=whtest&menuId=183f47fac3f745cdb3dda0ce9b197f90
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:---------------------------------------------------
 `mcid`      | 是       | 公众号 ID。
 `menuId`    | 是       | 菜单 ID。

Response:

正常示例：

```
{"errmsg":"ok","errcode":0}
```


### 公众号菜单排序

方法名：/mc/menu/order.json

调用方式：参见 API 调用签名规则

URL：https://你自已的域名或 IP + 端口/mc/menu/order.json

** 示例 **

Request:

```
POST /mc/menu/order.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: Application/x-www-form-urlencoded

menuIds=b9454156343e40e49431c899efc060ca&orders=1&menuIds=6ce0d22381b2413eb3a59b3ec755c1c3&orders=2&menuIds=1cc6939e54ec4b128b96ba41ba9f170e&orders=3
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:---------------------------------------------------
 `menuIds`    | 是       | 需要排序的菜单 ID，支持对 1 级和 2 级菜单进行排序。
 `orders`     | 是       | 菜单的显示顺序，1 级菜单的 orders 值，只在同级菜单中进行排序，2 级菜单中的 orders 值只在对应的 1 级菜单下进行排序。

Response:

正常示例：

```
{"errmsg":"ok","errcode":0}
```


## 接收消息

当 App 内用户向应用公众帐号发消息时，融云服务器将 POST 消息数据包（XML格式）到开发者配置的消息接收 URL 上。

* 融云服务器在五秒内收不到响应会断掉连接，并且重新发起请求，总共重试三次。
*	关于重试的消息排重，推荐使用 MsgID 排重。
*	假如开发者服务器无法保证在五秒内处理并回复，可以直接回复空字符串，融云服务器不会对此作任何处理，且不会发起重试。

http 请求方式：POST

请求接收方式：[参见 API 接收签名规则](public_service.html#API_接收签名规则)

### 文本消息格式

** XML 格式 **

```
<xml>
 <ToUserName><![CDATA[toUserId]]></ToUserName>
 <FromUserName><![CDATA[fromUserName]]></FromUserName>
 <CreateTime>134223445860</CreateTime>
 <MsgType><![CDATA[text]]></MsgType>
 <Content><![CDATA[content]]></Content>
 <MsgId>msgId</MsgID>
</xml>
```

** 参数说明 **

 名称           | 说明
:---------------|:----------------------
 `ToUserName`   | 公众号 Id
 `FromUserName` | 发送方用户 Id
 `CreateTime`   | 消息创建时间
 `MsgType`      | 消息类型，此处为 text
 `Content`      | 文本消息内容
 `MsgId`        | 消息 Id

### 图片消息格式

** XML 格式 **

```
<xml>
 <ToUserName><![CDATA[toUserName]]></ToUserName>
 <FromUserName><![CDATA[fromUserName]]></FromUserName>
 <CreateTime>134223445860</CreateTime>
 <MsgType><![CDATA[image]]></MsgType>
 <PicUrl><![CDATA[this is a url]]></PicUrl>
 <MsgId>msgId</MsgID>
</xml>
```

** 参数说明 **

 名称           | 说明
:---------------|:-----------------------
 `ToUserName`   | 公众号 Id
 `FromUserName` | 发送方用户 Id
 `CreateTime`   | 消息创建时间
 `MsgType`      | 消息类型，此处为 image
 `PicUrl`       | 图片链接
 `MsgId`        | 消息 Id

### 语音消息格式

** XML 格式 **

```
<xml>
 <ToUserName><![CDATA[toUserName]]></ToUserName>
 <FromUserName><![CDATA[fromUserName]]></FromUserName>
 <CreateTime>134223445860</CreateTime>
 <MsgType><![CDATA[voice]]></MsgType>
 <VoiceUrl><![CDATA[this is url]]></VoiceUrl>
 <Format><![CDATA[AMR]]></Format>
 <MsgId>msgId</MsgID>
</xml>
```

** 参数说明 **

 名称           | 说明
:---------------|:----------------------------
 `ToUserName`   | 公众号 Id
 `FromUserName` | 发送方用户 Id
 `CreateTime`   | 消息创建时间
 `MsgType`      | 消息类型，此处为 voice
 `VoiceUrl`     | 语音链接
 `Format`       | 语音格式，目前支持 AMR 格式
 `MsgId`        | 消息 Id

### 位置消息格式

** XML 格式 **

```
<xml>
 <ToUserName><![CDATA[toUserName]]></ToUserName>
 <FromUserName><![CDATA[fromUserName]]></FromUserName>
 <CreateTime>134223445860</CreateTime>
 <MsgType><![CDATA[location]]></MsgType>
 <Location_X>15.501</Location_X>
 <Location_Y>142.324</Location_Y>
 <Label><![CDATA[POI信息]]></Label>
 <MsgId>msgId</MsgID>
</xml>
```

** 参数说明 **

 名称           | 说明
:---------------|:--------------------------
 `ToUserName`   | 公众号 Id
 `FromUserName` | 发送方用户 Id
 `CreateTime`   | 消息创建时间
 `MsgType`      | 消息类型，此处为 location
 `Location_X`   | 纬度
 `Location_Y`   | 经度
 `Label`        | 地理位置信息
 `MsgId`        | 消息 Id

 ### 图文消息格式

 ** XML 格式 **

 ```
 <xml>
  <ToUserName><![CDATA[toUserName]]></ToUserName>
  <FromUserName><![CDATA[fromUserName]]></FromUserName>
  <CreateTime>134223445860</CreateTime>
  <MsgType><![CDATA[imgtxt]]></MsgType>
  <Title><![CDATA[title]]></Title>
  <Description><![CDATA[description]]></Description>
  <PicUrl><![CDATA[picUrl]]></PicUrl>
  <Url><![CDATA[url]]></Url>
  <MsgId>msgId</MsgID>
 </xml>
 ```

 ** 参数说明 **

  名称           | 说明
 :---------------|:--------------------------
  `ToUserName`   | 开发者帐号
  `FromUserName` | 发送方帐号
  `CreateTime`   | 消息创建时间
  `MsgType`      | 消息类型，此处为 imgtxt
  `Title`        | 消息的标题
  `Description`  | 消息文本内容
  `PicUrl`       | 图片地址
  `Url`          | 跳转的地址
  `MsgId`        | 消息 Id


## 事件推送

### 关注事件

用户在关注公众号时，会把这个事件消息推送到开发者置设的消息接收 URL 上。方便开发者给用户下发消息或做帐号的解绑操作。

* 融云服务器在 五 秒内收不到响应会断掉连接，并且重新发起请求，总共重试 三 次。
*	假如开发者服务器无法保证在 五 秒内处理并回复，可以直接回复空字符串，融云服务器不会对此作任何处理，且不会发起重试。

** XML 消息格式 **

```
<xml>
 <ToUserName><![CDATA[toUserName]]></ToUserName>
 <FromUserName><![CDATA[fromUserName]]></FromUserName>
 <CreateTime>134223445860</CreateTime>
 <MsgType><![CDATA[event]]></MsgType>
 <Event><![CDATA[subscribe]]></Event>
</xml>
```

** 参数说明 **

 名称           | 说明
:---------------|:--------------------------
 `ToUserName`   | 公众号 Id
 `FromUserName` | 发送方用户 Id
 `CreateTime`   | 消息创建时间
 `MsgType`      | 消息类型，此务为event
 `Event`        | 事件信息，此处为subscribe

### 取消关注事件

用户在关注/取消关注公众号时，会把这个事件消息推送到开发者置设的消息接收 URL 上。方便开发者给用户下发消息或做帐号的解绑操作。

* 融云服务器在 五 秒内收不到响应会断掉连接，并且重新发起请求，总共重试 三 次。
*	假如开发者服务器无法保证在 五 秒内处理并回复，可以直接回复空字符串，融云服务器不会对此作任何处理，且不会发起重试。

** XML 消息格式 **

```
<xml>
 <ToUserName><![CDATA[toUserName]]></ToUserName>
 <FromUserName><![CDATA[fromUserName]]></FromUserName>
 <CreateTime>134223445860</CreateTime>
 <MsgType><![CDATA[event]]></MsgType>
 <Event><![CDATA[unsubscribe]]></Event>
</xml>
```

** 参数说明 **

 名称           | 说明
:---------------|:----------------------------
 `ToUserName`   | 公众号 Id
 `FromUserName` | 发送方用户 Id
 `CreateTime`   | 消息创建时间
 `MsgType`      | 消息类型，此务为event
 `Event`        | 事件信息，此处为unsubscribe

## 关注、取消关注公众号

### 关注公众号

用户关注公众号接口，支持多个用户同时关注某一公众号，通过接口关注公众号时不进行关注事件的推送。

方法名：/mc/subscribe.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/mc/subscribe.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`

** 示例 **

Request:

```
POST /mc/subscribe.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

 {
    "mcId":"mcid001",
    "userId":["userId1","userId2"]
 }
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:---------------------------------------------
 `mcId`       | 是       | 公众号 Id。
 `userId`     | 是       | 关注公众号用户 Id 列表，最多支持 50 个用户同时关注。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 取消关注公众号

用户取消关注公众号接口，支持多个用户同时取消关注某一公众号，通过接口取消关注公众号时不进行取消关注事件的推送。

方法名：/mc/unsubscribe.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/mc/unsubscribe.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`

** 示例 **

Request:

```
POST /mc/unsubscribe.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

 {
    "mcId":"mcid001",
    "userId":["userId1","userId2"]
 }
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:---------------------------------------------
 `mcId`       | 是       | 公众号 Id。
 `userId`     | 是       | 取消关注公众号用户 Id 列表，最多支持 50 个用户同时取消关注。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

## 发送消息-使用已上传素材发送

用户向公众号发送消息后，公众号会向用户发送回复消息，目前融云公众服务 API 支持文本、图片、语音、图文等消息类型。

请注意，在发送多媒体（图片、语音）消息时需要预先[上传多媒体文件](public_service.html#上传多媒体文件)到融云服务器。

方法名：/message/send.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/message/send.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`


### 文本消息-使用已上传素材发送

** 示例 **

Request:

```
POST /message/send.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":"sdfxl@uwd1c0sxdlx2",
    "msgtype":"text",
    "text":
    {
         "content":"Hello"
    }
}
```

表单参数：

名称         | 是否必须 | 说明
:-------------|:---------|:---------------------------------------------
`mcId`       | 是       | 公众号 Id，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
`userId`     | 是       | 关注公众号用户 Id 列表。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"invalid media_id"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 图片消息-使用已上传素材发送

** 示例 **

Request:

```
POST /message/send.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-imestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":"123@uwd1c0sxdlx2",
    "msgtype":"image",
    "image":
    {
          "media_id":"123434343443"
    }
 }
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:-----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`123@uwd1c0sxdlx2` 。
 `msgtype`    | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 image。
 `media_id`   | 是       | 图片 Id，调用多媒体上传接口上传图片后获得。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"invalid media_id"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。


### 语音消息-使用已上传素材发送

** 示例 **

Request:

```
POST /message/send.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":"123@uwd1c0sxdlx2",
    "msgtype":"voice",
    "voice":
    {
          "media_id":"123434343443"
    }
}
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:-----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`123@uwd1c0sxdlx2` 。
 `msgtype`    | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 voice。
 `media_id`   | 是       | 语音 Id，调用多媒体上传接口上传语音后获得。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"invalid media_id"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 图文消息-使用已上传素材发送

** 示例 **

Request:

```
POST /message/send.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":"123@uwd1c0sxdlx2",
    "msgtype":"news",
    "news":
    {
        "articles": [
         {
             "title":"haha ",
             "description":" hahahaahhaha",
             "url":"URL",
             "picurl":"PIC_URL"
         },
         {
             "title":"lala",
             "description":" lalalalala",
             "url":"URL",
             "picurl":"PIC_URL"
         }
         ]
    }
}
```

表单参数：

 名称          | 是否必须 | 说明
:--------------|:---------|:----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`123@uwd1c0sxdlx2` 。
 `msgtype`     | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 news。
 `articles`    | 是       | 图文消息，一个图文消息最少为 1 条，最多为 10 条图文。
 `title`       | 是       | 图文消息的标题。
 `description` | 否       | 图文消息的描述，描述内容不超过 1000 个汉字( 1 个汉字等于 2 个字符)。
 `url`         | 否       | 图文消息被点击后跳转的链接。
 `picurl`      | 是       | 图文消息的图片链接，支持JPG。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"invalid media_id"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

## 群发消息-使用已上传素材发送

公众号需要向全部关注用户发送消息时，融云公众服务为公众号提供了每天 一 条的群发权限，按自然天计算，暂时只支持向全部关注用户发送消息。

请注意，在群发多媒体消息时需要预先[上传多媒体文件](public_service.html#上传多媒体文件)到融云服务器。

方法名：/message/sendall.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/message/sendall.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`

### 群发文本消息-使用已上传素发送

** 示例 **

Request:

```
POST /message/sendall.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
   "fromuser":"app.public.server112@uwd1c0sxdlx2",
   "filter":{
      "is_to_all": true
   },
   "text":{
      "content":"CONTENT"
   },
   "msgtype":"text"
}
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:----------------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `filter`    | 是       | 用于设定图文消息的接收者。
 `is_to_all` | 否       | 用于设定是否向全部用户发送，值为 true 或 false，目前暂只支持 true，默认为 true。
 `content`   | 是       | 消息内容，最大长度为 2000 个字节。
 `msgtype`   | 是       | 消息类型，图文消息为 mpnews，文本消息为 text，语音为 voice，图片为 image，此处为 text。

Response:

正常示例：

```
{
   "errcode":0,
   "errmsg":"ok",
   "msg_id":"adf330werfewfcxsdls"
}
```

错误示例：

```
{"errcode":1002,"errmsg":"media_id"}
```

可根据返回错误码，在 [API 返回状态码说明](public_service.html#API_返回状态码说明) 中查看错误明细。

### 群发图文消息-使用已上传素材发送

** 示例 **

Request:

```
POST /message/sendall.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
   "fromuser":"app.public.server112@uwd1c0sxdlx2",
   "filter":{
      "is_to_all":true
   },
   "mpnews":{
      "media_id":"123dsdajkasd231jhksad"
   },
   "msgtype":"mpnews"
}
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:-----------------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `filter`    | 是       | 用于设定图文消息的接收者。
 `is_to_all` | 否       | 用于设定是否向全部用户发送，值为 true 或 false，目前暂只支持 true，默认为 true。
 `msgtype`   | 是       | 消息类型，图文消息为mpnews，文本消息为 text，语音为 voice，图片为 image，此处为 mpnews。

Response:

正常示例：

```
{
   "errcode":0,
   "errmsg":"ok",
   "msg_id":"adf330werfewfcxsdls"
}
```

错误示例：

```
{"errcode":1002,"errmsg":"media_id"}
```

可根据返回错误码，在 [API 返回状态码说明](public_service.html#API_返回状态码说明) 中查看错误明细。

<div class="bs-callout bs-callout-info">
注意图文消息的 `media_id` 需要通过上传图文消息素材方法来得到。
</div>


### 群发语音消息-使用已上传素材发送

** 示例 **

Request:

```
POST /message/sendall.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
   "fromuser":"app.public.server112@uwd1c0sxdlx2",
   "filter":{
      "is_to_all": true
   },
   "voice":{
      "media_id":"123dsdajkasd231jhksad"
   },
   "msgtype":"voice"
}
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:-----------------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `filter`    | 是       | 用于设定图文消息的接收者。
 `is_to_all` | 否       | 用于设定是否向全部用户发送，值为 true 或 false，目前暂只支持true，默认为 true。
 `media_id`  | 是       | 用于群发的消息的 media_id。
 `msgtype`   | 是       | 消息类型，图文消息为 mpnews，文本消息为 text，语音为 voice，图片为 image，此处为 voice。

Response:

正常示例：

```
{
   "errcode":0,
   "errmsg":"ok",
   "msg_id":"adf330werfewfcxsdls"
}
```

错误示例：

```
{"errcode":1002,"errmsg":"media_id"}
```

可根据返回错误码，在 [API 返回状态码说明](public_service.html#API_返回状态码说明) 中查看错误明细。

<div class="bs-callout bs-callout-info">
注意此处 `media_id` 需通过上传多媒体文件方法来得到。
</div>


### 群发图片消息-使用已上传素材发送

** 示例 **

Request:

```
POST /message/sendall.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
   "fromuser":"app.public.server112@uwd1c0sxdlx2",
   "filter":{
      "is_to_all": true
   },
   "image":{
      "media_id":"123dsdajkasd231jhksad"
   },
   "msgtype":"image"
}
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:-----------------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `filter`    | 是       | 用于设定图文消息的接收者。
 `is_to_all` | 否       | 用于设定是否向全部用户发送，值为 true 或 false，目前暂只支持 true，默认为 true。
 `media_id`  | 是       | 用于群发的消息的 media_id。
 `msgtype`   | 是       | 消息类型，图文消息为 mpnews，文本消息为 text，语音为 voice，图片为 image，此处为 image。

Response:

正常示例：

```
{
   "errcode":0,
   "errmsg":"ok",
   "msg_id":"adf330werfewfcxsdls"
}
```

错误示例：

```
{"errcode":1002,"errmsg":"media_id"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

<div class="bs-callout bs-callout-info">
注意此处 `media_id` 需通过上传多媒体文件方法来得到。
</div>

## 素材管理

公众号在发送消息时经常用到一些媒体素材，需要开发者在上传多媒体文件后，通过返回的 media_id 进行发送。

### 上传多媒体文件

http 请求方式: POST

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则),不同点是将放进 http header 中的参数放到 URL 后面，如下：

```
http://你自已的域名或 IP + 端口/media/upload?type=TYPE&RC-App-Key=xxx&RC-Nonce=adbc134sdf&RC-Timestamp=1234556566&RC-Signature=123421dsfsvc03u4asdflvjdsdfsdf
```

表单参数：

 名称   | 是否必须 | 说明
:-------|:---------|:--------------------------------------------------------------
 `type` | 是       | 媒体文件类型，有图片（image）、语音（voice）、缩略图(thumb)。

上传的多媒体文件以表单文件方式上传，格式和大小限制如下：
*	图片（image）: 2 M，支持 JPG、PNG 格式。
*	缩略图(thumb) : 20 K，支持JPG、PNG格式。
*	语音（voice）：60 k，播放长度不超过 60s，目前支持 AMR 格式。


Java 代码示例：

```Java
String urlStr = "http://你自已的域名或 IP + 端口/media/upload?type=image&RC-App-Key=xxx&RC-Nonce=adbc134sdf&RC-Timestamp=1234556566&RC-Signature=123421dsfsvc03u4asdflvjdsdfsdf";

		//
		HttpClient httpclient = new DefaultHttpClient();
		// 请求处理页面
		 HttpPost httppost = new HttpPost(urlStr);

		// 创建待处理的文件
		FileBody file = new FileBody(new File(filepath));

		// 对请求的表单域进行填充
		MultipartEntity reqEntity = new MultipartEntity();
		reqEntity.addPart("file", file);

		// 设置请求
		httppost.setEntity(reqEntity);
		// 执行
		HttpResponse response = httpclient.execute(httppost);

		HttpEntity httpEntity = response.getEntity();
		BufferedReader br = new BufferedReader(new InputStreamReader(httpEntity.getContent(), "UTF-8"));
		StringBuffer backData = new StringBuffer();
		String line = null;
		while ((line = br.readLine()) != null) {
			backData.append(line);
		}
		System.out.println(backData.toString());
```

** 返回值 **

正常示例：

```
{
   "type":"image",
   "media_id":"12344343252",
   "created_at":123456789
}
```

错误示例：

```
{"errcode":1003,"errmsg":"invalid type "}
```

可根据返回错误码，在 [API 返回状态码说明](public_service.html#API_返回状态码说明) 中查看错误明细。

### 上传图文消息素材

http 请求方式: POST

调用方式：[参见 API 调用签名规则](public_service.html#API调用签名规则),不同点是将放进 http header 中的参数放到 URL 后面，如下：

```
http://你自已的域名或 IP + 端口/media/uploadnews?RC-App-Key=xxx&RC-Nonce=adbc134sdf&RC-Timestamp=1234556566&RC-Signature=123421dsfsvc03u4asdflvjdsdfsdf
```

POST 数据示例：

```
{
   "articles": [
		 {
        "thumb_media_id":"sdflsdfjxcl324324sdfsdlfsd",
        "author":"yyuy",
			  "title":"Happy ",
			  "content_source_url":"www.rongcloud.com",
			  "content":"content",
			  "digest":"digest",
        "show_cover_pic":"1"
		 },
		 {
        "thumb_media_id":" sdflsdfjxcl324324sdfsdlfsdasdfsdf",
        "author":"yy",
			  "title":"Happy",
			  "content_source_url":"www.rongcloud.com",
			  "content":"content",
			  "digest":"digest",
        "show_cover_pic":"0"
		 }
   ]
}
```

表单参数：

 名称                 | 是否必须 | 说明
:---------------------|:---------|:---------------------------------------------------------------------
 `articles`           | 是       | 图文消息，一个图文消息支持 1 到 10 条图文。
 `thumb_media_id`     | 是       | 图文消息缩略图的 media_id，可以在基础支持-上传多媒体文件接口中获得。
 `author`             | 否       | 图文消息的作者。
 `title`              | 否       | 图文消息的标题。
 `content_source_url` | 是       | 在图文消息页面点击“阅读原文”后链接的页面，如内容中含有 & 字符，需要进行 UrlEncode 处理。
 `content`            | 否       | 图文消息页面的内容，支持 HTML 标签。
 `digest`             | 否       | 图文消息的描述。
 `show_cover_pic`     | 否       | 是否显示封面，1 为显示，0 为不显示。

** 返回值 **

```
{
   "type":"news",
   "media_id":"CsEf3ldqkAYJAU6EJeIkStVDSvffUJ54vqbThMgplD-VJXXof6ctX5fI6-aYyUiQ",
   "created_at":1391857799
}
```

表单参数：

 名称         | 说明
:-------------|:----------------------------------------
 `type`       | 媒体文件类型， news，即图文消息。
 `media_id`   | 媒体文件/图文消息上传后获取的唯一标识。
 `created_at` | 媒体文件上传时间。

## 发送消息

除了使用已上传的素材发送消息外，同时支持不使用已上传的素材，直接发送文本、图片、语音、图文等消息。

方法名：/message/operation/send.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/message/operation/send.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`

### 文本消息

** 示例 **

Request:

```
POST /message/operation/send.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":"sdfxl@uwd1c0sxdlx2",
    "msgtype":"text",
    "text":
    {
         "content":"Hello"
    }
 }
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`sdfxl@uwd1c0sxdlx2` 。
 `msgtype`    | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 text。
 `content`    | 是       | 消息内容，最大长度为 2000 个字节。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 图片消息

** 示例 **

Request:

```
POST /message/operation/send.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-imestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":"123@uwd1c0sxdlx2",
    "msgtype":"image",
    "image":
    {
          "url":"http://www.xxx.com/aaa.jpg"
    }
 }
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:-----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`123@uwd1c0sxdlx2` 。
 `msgtype`    | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 image。
 `url`        | 是       | 图片 URL 地址，支持 JPG。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 语音消息

** 示例 **

Request:

```
POST /message/operation/send.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":"123@uwd1c0sxdlx2",
    "msgtype":"voice",
    "voice":
    {
          "url":"http://www.xxx.com/aaa.amr"
    }
}
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:-----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`123@uwd1c0sxdlx2` 。
 `msgtype`    | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 voice。
 `url`        | 是       | 语音文件 URL 地址，支持 AMR 文件。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 图文消息

** 示例 **

Request:

```
POST /message/operation/send.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":"123@uwd1c0sxdlx2",
    "msgtype":"news",
    "news":
    {
        "articles": [
         {
             "title":"haha ",
             "description":" hahahaahhaha",
             "url":"URL",
             "picurl":"PIC_URL"
         },
         {
             "title":"lala",
             "description":" lalalalala",
             "url":"URL",
             "picurl":"PIC_URL"
         }
         ]
    }
}
```

表单参数：

 名称          | 是否必须 | 说明
:--------------|:---------|:----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`123@uwd1c0sxdlx2` 。
 `msgtype`     | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 news。
 `articles`    | 是       | 图文消息，一个图文消息最少为 1 条，最多为 10 条图文。
 `title`       | 是       | 图文消息的标题。
 `description` | 否       | 图文消息的描述，描述内容不超过 1000 个汉字( 1 个汉字等于 2 个字符)。
 `url`         | 否       | 图文消息被点击后跳转的链接。
 `picurl`      | 是       | 图文消息的图片链接，支持 JPG。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

## 批量发送消息

### 批量发送文本消息

方法名：/message/batchsend.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/message/batchsend.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`

** 示例 **

Request:

```
POST /message/batchsend.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":["gDNQhlPfX@0vnjpoanq0ymg","BzUPcKM2B@0vnjpoanq0ymg"],
    "msgtype":"text",
    "text":
    {
         "content":"Hello"
    }
 }
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`sdfxl@uwd1c0sxdlx2` ，一次最多发送 100 个用户。
 `msgtype`    | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 text。
 `content`    | 是       | 消息内容，最大长度为 2000 个字节。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 批量发送图片消息

方法名：/message/operation/batchsend.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/message/operation/batchsend.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`

** 示例 **

Request:

```
POST /message/operation/batchsend.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-imestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":["gDNQhlPfX@0vnjpoanq0ymg","BzUPcKM2B@0vnjpoanq0ymg"],
    "msgtype":"image",
    "image":
    {
          "url":"http://www.xxx.com/aaa.jpg"
    }
 }
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:-----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`123@uwd1c0sxdlx2` ，一次最多发送 100 个用户。
 `msgtype`    | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 image。
 `url`        | 是       | 图片 URL 地址，支持 JPG。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 批量发送语音消息

方法名：/message/operation/batchsend.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/message/operation/batchsend.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`

** 示例 **

Request:

```
POST /message/operation/batchsend.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":["gDNQhlPfX@0vnjpoanq0ymg","BzUPcKM2B@0vnjpoanq0ymg"],
    "msgtype":"voice",
    "voice":
    {
          "url":"http://www.xxx.com/aaa.amr"
    }
}
```

表单参数：

 名称         | 是否必须 | 说明
:-------------|:---------|:-----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`123@uwd1c0sxdlx2` ，一次最多发送 100 个用户。
 `msgtype`    | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 voice。
 `url`        | 是       | 语音文件 URL 地址，支持 AMR 文件。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

### 批量发送图文消息

方法名：/message/batchsend.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/message/batchsend.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`

** 示例 **

Request:

```
POST /message/batchsend.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
    "fromuser":"app.public.server112@uwd1c0sxdlx2",
    "touser":["gDNQhlPfX@0vnjpoanq0ymg","BzUPcKM2B@0vnjpoanq0ymg"],
    "msgtype":"news",
    "news":
    {
        "articles": [
         {
             "title":"haha ",
             "description":" hahahaahhaha",
             "url":"URL",
             "picurl":"PIC_URL"
         },
         {
             "title":"lala",
             "description":" lalalalala",
             "url":"URL",
             "picurl":"PIC_URL"
         }
         ]
    }
}
```

表单参数：

 名称          | 是否必须 | 说明
:--------------|:---------|:----------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `touser`     | 是       | 接收用户标识，格式：`用户 Id` + @ + `App key` 例如：`123@uwd1c0sxdlx2` ，一次最多发送 100 个用户。
 `msgtype`     | 是       | 消息类型，文本为 text，图片为 image，语音为 voice，图文消息为 news，此处为 news。
 `articles`    | 是       | 图文消息，一个图文消息最少为 1 条，最多为 10 条图文。
 `title`       | 是       | 图文消息的标题。
 `description` | 否       | 图文消息的描述，描述内容不超过 1000 个汉字( 1 个汉字等于 2 个字符)。
 `url`         | 否       | 图文消息被点击后跳转的链接。
 `picurl`      | 是       | 图文消息的图片链接，支持 JPG。

Response:

正常示例：

```
{ "errcode" : 0,"errmsg" : "ok" }
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。

## 群发消息

除了使用已上传的素材发送消息外，同时支持不使用已上传的素材，直接发送文本、图片、语音、图文等消息。

方法名：/message/operation/sendall.json

调用方式：[参见 API 调用签名规则](public_service.html#API_调用签名规则)

URL：https://你自已的域名或 IP + 端口/message/operation/sendall.json

请注意：发送请求时 HTTP Request Header 中的 `Content-Type` 类型必须为 `application/json`

### 群发文本消息

** 示例 **

Request:

```
POST /message/operation/sendall.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
   "fromuser":"app.public.server112@uwd1c0sxdlx2",
   "text":{
      "content":"CONTENT"
   },
   "msgtype":"text"
}
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:----------------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `content`   | 是       | 消息内容，最大长度为 2000 个字节。
 `msgtype`   | 是       | 消息类型，图文消息为 mpnews，文本消息为 text，语音为 voice，图片为 image，此处为 text。

Response:

正常示例：

```
{
   "errcode":0,
   "errmsg":"ok",
   "msg_id":"adf330werfewfcxsdls"
}
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 [API 返回状态码说明](public_service.html#API_返回状态码说明) 中查看错误明细。

### 群发图文消息

** 示例 **

Request:

```
POST /message/operation/sendall.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
   "fromuser":"app.public.server112@uwd1c0sxdlx2",
   "news":{
     "articles": [
      {
          "title":"haha ",
          "description":" hahahaahhaha",
          "url":"URL",
          "picurl":"PIC_URL"
      },
      {
          "title":"lala",
          "description":" lalalalala",
          "url":"URL",
          "picurl":"PIC_URL"
      }
      ]
   },
   "msgtype":"news"
}
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:-----------------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `msgtype`   | 是       | 消息类型，图文消息为 news，文本消息为 text，语音为 voice，图片为 image，此处为 news。
 `articles`    | 是       | 图文消息，一个图文消息最少为 1 条，最多为 10 条图文。
 `title`       | 是       | 图文消息的标题。
 `description` | 否       | 图文消息的描述，描述内容不超过 1000 个汉字( 1 个汉字等于 2 个字符)。
 `url`         | 否       | 图文消息被点击后跳转的链接。
 `picurl`      | 是       | 图文消息的图片链接，支持 JPG。

Response:

正常示例：

```
{
   "errcode":0,
   "errmsg":"ok",
   "msg_id":"adf330werfewfcxsdls"
}
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 [API 返回状态码说明](public_service.html#API_返回状态码说明) 中查看错误明细。

### 群发语音消息

** 示例 **

Request:

```
POST /message/operation/sendall.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
   "fromuser":"app.public.server112@uwd1c0sxdlx2",
   "voice":{
      "url":"http://www.xxx.com/aaa.amr"
   },
   "msgtype":"voice"
}
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:-----------------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `url`       | 是       | 语音文件地址，支持 AMR 格式。
 `msgtype`   | 是       | 消息类型，图文消息为 mpnews，文本消息为 text，语音为 voice，图片为 image，此处为 voice。

Response:

正常示例：

```
{
   "errcode":0,
   "errmsg":"ok",
   "msg_id":"adf330werfewfcxsdls"
}
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 [API 返回状态码说明](public_service.html#API_返回状态码说明) 中查看错误明细。

### 群发图片消息

** 示例 **

Request:

```
POST /message/operation/sendall.json HTTP/1.1
Host: 你自已的域名或 IP + 端口
RC-App-Key: uwd1c0sxdlx2
RC-Nonce: 14314
RC-Timestamp: 1408706337
RC-Signature: 890b422b75c1c5cb706e4f7921df1d94e69c17f4
Content-Type: application/json

{
   "fromuser":"app.public.server112@uwd1c0sxdlx2",
   "image":{
      "url":"http://www.xxxx.com/aaa.jpg"
   },
   "msgtype":"image"
}
```

表单参数：

 名称        | 是否必须 | 说明
:------------|:---------|:-----------------------------------------------------------------------------------------
 `fromuser`   | 是       | 发送用户标识，格式：`公众号 Id` + @ + `App key` 例如：`app.public.server112@uwd1c0sxdlx2` 。
 `url`       | 是       | 图片文件地址，支持 JPG 格式。
 `msgtype`   | 是       | 消息类型，图文消息为 mpnews，文本消息为 text，语音为 voice，图片为 image，此处为 image。

Response:

正常示例：

```
{
   "errcode":0,
   "errmsg":"ok",
   "msg_id":"adf330werfewfcxsdls"
}
```

错误示例：

```
{"errcode":1002,"errmsg":"参数错误"}
```

可根据返回错误码，在 API 返回状态码说明中查看错误明细。


## API 返回状态码说明

### 业务返回码
 code | 描述         | 详细解释
:-----|:-------------|:---------------------------------------
 1000 | 服务内部错误 | 服务器端内部逻辑错误,请稍后重试
 1002 | 参数错误     | 参数错误，详细的描述信息会说明
 1004 | 验证签名错误 | 验证签名错误
 1007 | 被限制调用   | 该方法被限制调用，详细的描述信息会说明
 1008 | 调用频率超限 | 调用频率超限，详细的描述信息会说明

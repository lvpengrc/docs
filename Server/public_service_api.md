# 应用公众服务开发文档

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

 &emsp;&emsp;RC-Signature (数据签名)计算方法：将系统分配的 App Secret、RC-Nonce (随机数)、RC-Timestamp (时间戳)三个字符串按先后顺序拼接成一个字符串并进行 SHA1 哈希计算。如果调用的数据签名验证失败，接口调用会返回 HTTP 状态码 `401`。

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


## 发送消息

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


## 群发消息

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

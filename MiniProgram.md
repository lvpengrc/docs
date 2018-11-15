# 小程序解决方案

融云小程序解决方案，咨询电话：13161856839

## 体验小程序

使用微信扫描下面二维码，体验融云小程序功能。

![二维码](img/xiaochengxu_code.png)

> 小程序体验版只包含部分 IM 功能，详细参考`功能介绍`。

##  功能介绍

面向微信小程序的 IM SDK，为小程序提供安全稳定的 IM 能力，集成简单，使用方便，帮助你快速拓展业务，赢得先机。

|分类	   	| 功能描述
|:--------	| :----------------------------------------------
|会话模式	   	| 支持单聊、群聊、聊天室、客服
|会话列表	   	| 按会话中最新一条消息时间倒序显示，支持会话删除、获取会话未读消息数功能。
|消息类型 	| 提供[内置内容类消息、通知类消息、状态类消息](http://www.rongcloud.cn/docs/web_api_demo.html#message_type)、[自定义消息](http://www.rongcloud.cn/docs/web_api_demo.html#message_customize)
|消息操作		| 支持消息撤回、获取历史消息记录、清空单群聊历史消息
|群组		| 提供创建、加入、退出、解散、群成员禁言等功能，详细查看[群组业务说明](http://www.rongcloud.cn/docs/web_api_demo.html#group)及 [Server API](http://rongcloud.cn/docs/server.html#group) 功能介绍
|聊天室		| 提供创建、加入、退出、销毁聊天室、聊天室禁言等基础功能，及聊天室消息优先级、聊天室用户白名单、聊天室消息云存储等高级服务。查看[付费服务指南](http://www.rongcloud.cn/docs/payment.html)及 [Server API](http://www.rongcloud.cn/docs/server.html#chatroom) 功能介绍
|安全审核		| [安全审核产品](http://www.rongcloud.cn/product/antispam)及 [Server API](http://rongcloud.cn/docs/server.html#sensitiveword) 功能介绍


##  快速集成

步骤如下：

![集成流程](img/xiaochengxu.png)

** 步骤说明： **

1. 注册为融云开发者
	
	前往[融云官方网站](http://www.rongcloud.cn?_blank)注册创建融云开发者帐号。
	
2. 创建应用
	
	获取 App Key / Secret，它们是融云 SDK 连接服务器所必须的标识，每一个应用对应一套 App Key / Secret。针对开发者的生产环境和开发环境，提供两套 App Key / Secret，两套环境的功能完全一致。应用最终上线前，使用开发环境即可。
	
3. 引入 SDK

4. 集成调试

5. 应用上线，上线前需要做以下操作：
	
	1）提前告知融云当前小程序日活数据，以便融云为您提供更稳定的服务。
	
	2）在微信小程序后台，设置合法域名：
	
		request 合法域名：https://nav.cn.ronghub.com
		
		socket 合法域名：wss://wsproxy.cn.ronghub.com
	
	3）融云平台申请上线，获取生产环境配置信息，申请上线前必须在融云开发环境下实现了发送消息测试。查看[上线注意事项](http://support.rongcloud.cn/kb/MzMw)

## 初始化

### 初始化 SDK

执行初始化需要在开发者后台新建应用得到 `AppKey` 和 `token`，初始化代码：

```js
RongIMLib.RongIMClient.init(appkey,[dataAccessProvider],[options])
```

AppKey：应用的唯一标识。

Token ：用户的唯一标识。

```js
RongIMLib.RongIMClient.init("appkey");
```

### 设置监听

1、必须设置监听器后，再连接融云服务器，示例代码如下：

```js
// 设置连接监听状态 （ status 标识当前连接状态 ）
 // 连接状态监听器
 RongIMClient.setConnectionStatusListener({
    onChanged: function (status) {
        switch (status) {
            case RongIMLib.ConnectionStatus.CONNECTED:
                console.log('链接成功');
                break;
            case RongIMLib.ConnectionStatus.CONNECTING:
                console.log('正在链接');
                break;
            case RongIMLib.ConnectionStatus.DISCONNECTED:
                console.log('断开连接');
                break;
            case RongIMLib.ConnectionStatus.KICKED_OFFLINE_BY_OTHER_CLIENT:
                console.log('其他设备登录');
                break;
  			case RongIMLib.ConnectionStatus.DOMAIN_INCORRECT:
                console.log('域名不正确');
                break;
            case RongIMLib.ConnectionStatus.NETWORK_UNAVAILABLE:
              console.log('网络不可用');
              break;
            }
    }});

 // 消息监听器
 RongIMClient.setOnReceiveMessageListener({
    // 接收到的消息
    onReceived: function (message) {
        // 判断消息类型
        switch(message.messageType){
            case RongIMClient.MessageType.TextMessage:
                // message.content.content => 消息内容
                break;
            case RongIMClient.MessageType.VoiceMessage:
                // 对声音进行预加载                
                // message.content.content 格式为 AMR 格式的 base64 码
                break;
            case RongIMClient.MessageType.ImageMessage:
			   // message.content.content => 图片缩略图 base64。
			   // message.content.imageUri => 原图 URL。
               break;
            case RongIMClient.MessageType.DiscussionNotificationMessage:
               // message.content.extension => 讨论组中的人员。
               break;
            case RongIMClient.MessageType.LocationMessage:
               // message.content.latiude => 纬度。
			   // message.content.longitude => 经度。
			   // message.content.content => 位置图片 base64。
               break;
            case RongIMClient.MessageType.RichContentMessage:
               // message.content.content => 文本消息内容。
			   // message.content.imageUri => 图片 base64。
               // message.content.url => 原图 URL。
               break;
            case RongIMClient.MessageType.InformationNotificationMessage:
                // do something...
               break;
            case RongIMClient.MessageType.ContactNotificationMessage:
                // do something...
               break;
            case RongIMClient.MessageType.ProfileNotificationMessage:
                // do something...
               break;
            case RongIMClient.MessageType.CommandNotificationMessage:
                // do something...
               break;
            case RongIMClient.MessageType.CommandMessage:
                // do something...
               break;
            case RongIMClient.MessageType.UnknownMessage:
                // do something...
               break;
            default:
                // do something...
        }
    }
});
```
### 连接服务器

连接融云必须在执行 `RongIMLib.RongIMClient.init(appkey);` 之后，并且所有除监听以外的方法都必须在 `connect成功之后` 再调用。

```js
  var token = "mKmyKqTSf7aNDinwAFMnz7NXKILeV3X0+CCRBOxmtOApmvQjMathViWrePIfq0GuTu9jELQqsckv4AhfjCAKgQ==";

  RongIMClient.connect(token, {
        onSuccess: function(userId) {
          console.log("Connect successfully." + userId);
        },
        onTokenIncorrect: function() {
          console.log('token无效');
        },
        onError:function(errorCode){
              var info = '';
              switch (errorCode) {
                case RongIMLib.ErrorCode.TIMEOUT:
                  info = '超时';
                  break;
                case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                  info = '未知错误';
                  break;
                case RongIMLib.ErrorCode.UNACCEPTABLE_PaROTOCOL_VERSION:
                  info = '不可接受的协议版本';
                  break;
                case RongIMLib.ErrorCode.IDENTIFIER_REJECTED:
                  info = 'appkey不正确';
                  break;
                case RongIMLib.ErrorCode.SERVER_UNAVAILABLE:
                  info = '服务器不可用';
                  break;
              }
              console.log(errorCode);
            }
      });
```

### 重新连接

```js
    var callback = {
        onSuccess: function(userId) {
            console.log("Reconnect successfully." + userId);
        },
        onTokenIncorrect: function() {
            console.log('token无效');
        },
        onError:function(errorCode){
            console.log(errorcode);
        }
    };
    var config = {
        // 默认 false, true 启用自动重连，启用则为必选参数
        auto: true,
        // 重试频率 [100, 1000, 3000, 6000, 10000, 18000] 单位为毫秒，可选
        url: 'cdn.ronghub.com/RongIMLib-2.2.6.min.js',
        // 网络嗅探地址 [http(s)://]cdn.ronghub.com/RongIMLib-2.2.6.min.js 可选
        rate: [100, 1000, 3000, 6000, 10000]
    };
    RongIMClient.reconnect(callback, config);
```
## 消息接口

### 消息类型

消息分类   | 消息行为状态标识
:-----------|:---------------------------------------------------------------------------------------
内容类消息 | 表示一个用户间发送的包含具体内容的消息，需要展现在聊天界面上，如文字消息、语音消息等。
通知类消息 | 表示一个通知信息，可能展现在聊天界面上，如提示条通知。
状态类消息 | 表示一个状态，用来实现如“对方正在输入”的功能。

** 内容类消息 **

消息类型    | ObjectName  | 类名         | 是否计数   | 是否存储
:---------|:-------------|:-------------|:-----------|:----------|:-----------------------------
文字消息    | `RC:TxtMsg` | TextMessage  | 是      | 是
语音消息    | `RC:VcMsg`  | VoiceMessage | 是      | 是
图片消息    | `RC:ImgMsg` | ImageMessage | 是      | 是
图文消息    | `RC:ImgTextMsg` | RichContentMessage  | 是      | 是
位置消息    | `RC:LBSMsg` | LocationMessage  | 是      | 是
公众服务单图文消息    | `RC:PSImgTxtMsg` | PublicServiceRichContentMessage  | 是   | 是
公众服务多图文消息 | `RC:PSMultiImgTxtMsg` | PublicServiceMultiRichContentMessage  | 是      | 是

** 通知类消息 **

消息类型    | ObjectName  | 类名         | 是否计数   | 是否存储
:---------|:-------------|:------------|:----------|:-----------------------------
好友通知消息 | `RC:ContactNtf` | ContactNotificationMessage | 否     | 是
资料通知消息 | `RC:ProfileNtf` | ProfileNotificationMessage | 否     | 是
通用命令通知消息 | `RC:CmdNtf` | CommandNotificationMessage | 否     | 是
提示条通知消息 | `RC:InfoNtf` | InformationNotificationMessage | 否     | 是
群组通知消息 | `RC:GrpNtf` | GroupNotificationMessage | 否     | 是
讨论组通知消息 | `RC:DizNtf` | DiscussionNotificationMessage | 否     | 是
已读通知消息 | `RC:ReadNtf` | ReadReceiptMessage | 否     | 否
公众服务命令消息 | `RC:PSCmd` | PublicServiceCommandMessage | 否     | 否
命令消息 | `RC:CmdMsg` | CommandMessage | 否     | 否

** 状态类消息 **

消息类型    | ObjectName  | 类名         | 是否计数   | 是否存储
:---------|:-------------|:-------------|:----------|:-----------------------------
对方正在输入状态消息 | `RC:TypSts` | TypingStatusMessage | 否     | 否

<div class="bs-callout bs-callout-info">
是否计数：表示客户端收到消息后，是否进行未读消息计数（未读消息数增加 1），所有内容型消息都应该设置此值。
<br>
是否存储：表示客户端收到消息后，是否进行存储，并在之后可以通过接口查询。
</div>

### 发送消息

1、文本消息

```js
 var msg = new RongIMLib.TextMessage({content:"hello RongCloud!",extra:"附加信息"});
 var conversationtype = RongIMLib.ConversationType.PRIVATE; // 单聊,其他会话选择相应的消息类型即可。
 var targetId = "xxx"; // 目标 Id
 RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
                onSuccess: function (message) {
                    //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
                    console.log("Send successfully");
                },
                onError: function (errorCode,message) {
                    var info = '';
                    switch (errorCode) {
                        case RongIMLib.ErrorCode.TIMEOUT:
                            info = '超时';
                            break;
                        case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                            info = '未知错误';
                            break;
                        case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                            info = '在黑名单中，无法向对方发送消息';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                            info = '不在讨论组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_GROUP:
                            info = '不在群组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                            info = '不在聊天室中';
                            break;
                        default :
                            info = x;
                            break;
                    }
                    console.log('发送失败:' + info);
                }
            }
        );
```

发送 pushData[通知] 给端 Android or iOS 方法如下：

```js
 var msg = new RongIMLib.TextMessage({content:"hello RongCloud!",extra:"附加信息"});
 var conversationtype = RongIMLib.ConversationType.PRIVATE; // 单聊,其他会话选择相应的消息类型即可。
 var targetId = "xxx"; // 目标 Id
 var pushData = "your pushInfo";
 RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
        onSuccess: function (message) {
            .......
        },
        onError: function (errorCode,message) {
            ......
        }
    }, false, pushData
);
```

下述所有信息类型有关 pushData 用法同此，不再复述。

2、图片消息

图片上传请参考：[上传](./web.html#upload_widget?_blank)

```js
  /*
     图片转为可以使用 HTML5 的 FileReader 或者 canvas 也可以上传到后台进行转换。

     注意事项：
         1、缩略图必须是 base64 码的 jpg 图。
         2、不带前缀。
         3、大小不得超过 100kb。
   */
 var base64Str = "base64 格式缩略图";
 var imageUri = "图片 URL"; // 上传到自己服务器的 URL。
 var msg = new RongIMLib.ImageMessage({content:base64Str,imageUri:imageUri});
 var conversationtype = RongIMLib.ConversationType.PRIVATE; // 单聊,其他会话选择相应的消息类型即可。
 var targetId = "xxx"; // 目标 Id
 RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
                onSuccess: function (message) {
                    //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
                    console.log("Send successfully");
                },
                onError: function (errorCode,message) {
                    var info = '';
                    switch (errorCode) {
                        case RongIMLib.ErrorCode.TIMEOUT:
                            info = '超时';
                            break;
                        case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                            info = '未知错误';
                            break;
                        case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                            info = '在黑名单中，无法向对方发送消息';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                            info = '不在讨论组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_GROUP:
                            info = '不在群组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                            info = '不在聊天室中';
                            break;
                        default :
                            info = x;
                            break;
                    }
                    console.log('发送失败:' + info);
                }
            }
        );
```

3、富文本消息

图片上传请参考：[上传](./web.html#upload_widget?_blank)

```js
 var base64Str = "base64 格式缩略图";// 图片转为可以使用 HTML5 的 FileReader 或者 canvas 也可以上传到后台进行转换。
 var imageUri = "图片 URL"; // 上传到自己服务器的 URL。
 var title = "富文本消息标题";
 var msg = new RongIMLib.RichContentMessage({title:title,content:base64Str,imageUri:imageUri});
 var conversationtype = RongIMLib.ConversationType.PRIVATE; // 单聊,其他会话选择相应的消息类型即可。
 var targetId = "xxx"; // 目标 Id
 RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
                onSuccess: function (message) {
                    //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
                    console.log("Send successfully");
                },
                onError: function (errorCode,message) {
                    var info = '';
                    switch (errorCode) {
                        case RongIMLib.ErrorCode.TIMEOUT:
                            info = '超时';
                            break;
                        case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                            info = '未知错误';
                            break;
                        case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                            info = '在黑名单中，无法向对方发送消息';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                            info = '不在讨论组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_GROUP:
                            info = '不在群组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                            info = '不在聊天室中';
                            break;
                        default :
                            info = x;
                            break;
                    }
                    console.log('发送失败:' + info);
                }
            }
        );
```

4、位置消息

```js
 var latitude = "纬度";
 var longitude = "经度";
 var poi = "位置描述";
 var content = "位置缩略图";
 var msg = new RongIMLib.LocationMessage({latitude: longitude, longitude: longitude, poi: poi, content: content});
 var conversationtype = RongIMLib.ConversationType.PRIVATE; // 单聊,其他会话选择相应的消息类型即可。
 var targetId = "xxx"; // 目标 Id
 RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
                onSuccess: function (message) {
                    //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
                    console.log("Send successfully");
                },
                onError: function (errorCode,message) {
                    var info = '';
                    switch (errorCode) {
                        case RongIMLib.ErrorCode.TIMEOUT:
                            info = '超时';
                            break;
                        case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                            info = '未知错误';
                            break;
                        case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                            info = '在黑名单中，无法向对方发送消息';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                            info = '不在讨论组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_GROUP:
                            info = '不在群组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                            info = '不在聊天室中';
                            break;
                        default :
                            info = x;
                            break;
                    }
                    console.log('发送失败:' + info);
                }
            }
        );
```

5、命令消息

```js
 var data = "命令指令";//可以是一个对象,data = {cmd:update};
 var msg = new RongIMLib.CommandMessage({data:data});
 var conversationtype = RongIMLib.ConversationType.PRIVATE; // 单聊,其他会话选择相应的消息类型即可。
 var targetId = "xxx"; // 目标 Id
 RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
                onSuccess: function (message) {
                    //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
                    console.log("Send successfully");
                },
                onError: function (errorCode,message) {
                    var info = '';
                    switch (errorCode) {
                        case RongIMLib.ErrorCode.TIMEOUT:
                            info = '超时';
                            break;
                        case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                            info = '未知错误';
                            break;
                        case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                            info = '在黑名单中，无法向对方发送消息';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                            info = '不在讨论组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_GROUP:
                            info = '不在群组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                            info = '不在聊天室中';
                            break;
                        default :
                            info = x;
                            break;
                    }
                    console.log('发送失败:' + info);
                }
            }
        );
```

6、正在输入状态消息

```js
var conversationType = RongIMLib.ConversationType.PRIVATE;//只有单聊有正在输入状态。
var targetId = "xxx";
var msgName = "TextMessage";//用户正在输入的消息类型 VoiceMessage等。
RongIMClient.getInstance().sendTypingStatusMessage(conversationType, targetId, msgName, {
                onSuccess: function (message) {
                    console.log("Send successfully");
                },
                onError: function (errorCode,message) {
                    var info = '';
                    switch (errorCode) {
                        case RongIMLib.ErrorCode.TIMEOUT:
                            info = '超时';
                            break;
                        case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                            info = '未知错误';
                            break;
                        case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                            info = '在黑名单中，无法向对方发送消息';
                            break;
                        default :
                            info = x;
                            break;
                    }
                    console.log('发送失败:' + info);
                }
            });
```

7、已读通知消息

```js
 var messageUId = "消息唯一 Id";
 var lastMessageSendTime = "最后一条消息的发送时间";
 var type = "1";// 备用，默认赋值 1 即可。
 // 以上 3 个属性在会话的最后一条消息中可以获得。
 var msg = new RongIMLib.ReadReceiptMessage({ messageUId: messageUId, lastMessageSendTime: lastMessageSendTime, type: type });
 var conversationtype = RongIMLib.ConversationType.PRIVATE; // 单聊,其他会话选择相应的消息类型即可。
 var targetId = "xxx"; // 目标 Id
 RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
                onSuccess: function (message) {
                    //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
                    console.log("Send successfully");
                },
                onError: function (errorCode,message) {
                    var info = '';
                    switch (errorCode) {
                        case RongIMLib.ErrorCode.TIMEOUT:
                            info = '超时';
                            break;
                        case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                            info = '未知错误';
                            break;
                        case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                            info = '在黑名单中，无法向对方发送消息';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                            info = '不在讨论组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_GROUP:
                            info = '不在群组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                            info = '不在聊天室中';
                            break;
                        default :
                            info = x;
                            break;
                    }
                    console.log('发送失败:' + info);
                }
            }
        );
```

8、发送 @ 消息

支持 @ 消息会话类型：`RongIMLib.ConversationType.GROUP`、`RongIMLib.ConversationType.DISCUSSION`。

支持 @ 消息消息类型：`TextMessage`、`ImageMessage`、`VoiceMessage`、`RichContentMessage`、`LocationMessage`。

发送方式：`RongIMClient.getInstance().sendMessage(conversationType,targetId,msg,callback,true);`，请设置最后一个参数为 `true` 。

```js
// 群聊。
var conversationtype = RongIMLib.ConversationType.GROUP;
// 目标 Id
var targetId = "groupId";
// @ 消息对象
var mentioneds = new RongIMLib.MentionedInfo();
    // 全部：RongIMLib.MentionedType.ALL；部分：RongIMLib.MentionedType.PART
    mentioneds.type = RongIMLib.MentionedType.PART;
    // @ 的人
    mentioneds.userIdList = [];
var msg = new RongIMLib.TextMessage({content:"hello RongCloud!",extra:"附加信息",mentionedInfo:mentioneds});
RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
               onSuccess: function (message) {
                   //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
                   console.log("Send successfully");
               },
               onError: function (errorCode,message) {
                   var info = '';
                   switch (errorCode) {
                       case RongIMLib.ErrorCode.TIMEOUT:
                           info = '超时';
                           break;
                       case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                           info = '未知错误';
                           break;
                       case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                           info = '在黑名单中，无法向对方发送消息';
                           break;
                       case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                           info = '不在讨论组中';
                           break;
                       case RongIMLib.ErrorCode.NOT_IN_GROUP:
                           info = '不在群组中';
                           break;
                       case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                           info = '不在聊天室中';
                           break;
                       default :
                           info = x;
                           break;
                   }
                   console.log('发送失败:' + info);
               }
           },true);
```

9、发送文件消息

文件上传请参考：[上传](./web.html#upload_widget)

```js
 var name = 'RongIMLib.js',
     size = 1024,
     type = 'js',
     fileUrl = 'http://xxxx.xxx.xx/RongIMLib.js';
 var msg = new RongIMLib.FileMessage({ name: name, size: size, type: type, fileUrl: fileUrl});
 var conversationtype = RongIMLib.ConversationType.PRIVATE; // 单聊,其他会话选择相应的消息类型即可。
 var targetId = "xxx"; // 目标 Id
 RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
                onSuccess: function (message) {
                    //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
                    console.log("Send successfully");
                },
                onError: function (errorCode,message) {
                    var info = '';
                    switch (errorCode) {
                        case RongIMLib.ErrorCode.TIMEOUT:
                            info = '超时';
                            break;
                        case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                            info = '未知错误';
                            break;
                        case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                            info = '在黑名单中，无法向对方发送消息';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                            info = '不在讨论组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_GROUP:
                            info = '不在群组中';
                            break;
                        case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                            info = '不在聊天室中';
                            break;
                        default :
                            info = x;
                            break;
                    }
                    console.log('发送失败:' + info);
                }
            }
        );
```

### 发送群定向消息--send_directional_message--

此方法用于在群组和讨论组中给部分用户发送消息，其它用户不会收到这条消息，例如向群中部分用户发送消息阅读状态回执时可使用此功能。

注：群定向消息不存储到云端，通过“[单群聊消息云存储](advanced_features.html#history_message_cloud_storage?_blank)”服务无法获取到定向消息。

```js
var isMentiondMsg = false;//是否为 @消息，true 为 @消息 、false 为普通消息
var pushText = '';//Push 显示内容
var appData = ''//Push 通知时附加信息
var methodType = null
var opts = {
     userIds: ['userId1', 'userId2']//接收消息的用户 ID 列表
};
var conversationtype = RongIMLib.ConversationType.GROUP; // 群聊会话类型。
var targetId = "xxx"; // 目标 Id

var msg = new RongIMLib.TextMessage({content:"我是定向消息, 只有 userId1, userId2 能收到", extra:"附加信息"});

RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
    onSuccess: function (message) {
        //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
        console.log("Send successfully");
    },
    onError: function (errorCode,message) {
        var info = '';
        switch (errorCode) {
            case RongIMLib.ErrorCode.TIMEOUT:
                info = '超时';
                break;
            case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                info = '未知错误';
                break;
            case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                info = '在黑名单中，无法向对方发送消息';
                break;
            case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                info = '不在讨论组中';
                break;
            case RongIMLib.ErrorCode.NOT_IN_GROUP:
                info = '不在群组中';
                break;
            case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                info = '不在聊天室中';
                break;
            default :
                info = x;
                break;
        }
        console.log('发送失败:' + info);
    }
},isMentiondMsg, pushText, appData, methodType, opts);
```

### 获取单群聊历史消息--message_history--

1、获取单群聊历史消息

<div class="bs-callout bs-callout-warning">
该功能需要在开发者后台“应用/IM 服务/高级功能设置”中开通公有云专业版后，开启`单群聊消息云存储`功能即可使用，如没有开启则执行 onError 方法。
</div>

```js
var conversationType = RongIMLib.ConversationType.PRIVATE; //单聊,其他会话选择相应的消息类型即可。
var targetId = "xxx"; // 想获取自己和谁的历史消息，targetId 赋值为对方的 Id。
var timestrap = null; // 默认传 null，若从头开始获取历史消息，请赋值为 0 ,timestrap = 0;
var count = 20; // 每次获取的历史消息条数，范围 0-20 条，可以多次获取。
RongIMLib.RongIMClient.getInstance().getHistoryMessages(conversationType, targetId, timestrap, count, {
  onSuccess: function(list, hasMsg) {
   	// list => Message 数组。
	// hasMsg => 是否还有历史消息可以获取。
  },
  onError: function(error) {
    console.log("GetHistoryMessages,errorcode:" + error);
  }
});
```

### 过滤消息--message_filter--

此方法目前只支持按消息名称过滤 `聊天室` 中的消息。

```js
var messageNames = ['CommandMessage']; // 可以设置多个。
RongIMClient.getInstance().setFilterMessages(messageNames);
```

### 消息草稿--message_draft--

1、保存草稿

```js
var conversationType = RongIMLib.ConversationType.PRIVATE;
var targetId = "xxx";
var draft = "草稿信息";
RongIMClient.getInstance().saveTextMessageDraft(conversationType,targetId,draft);
```

2、获取草稿

```js
var conversationType = RongIMLib.ConversationType.PRIVATE;
var targetId = "xxx";
var draft = RongIMClient.getInstance().getTextMessageDraft(conversationType,targetId);
// draft => 草稿信息。
```

3、清除草稿

```js
var conversationType = RongIMLib.ConversationType.PRIVATE;
var targetId = "xxx";
RongIMClient.getInstance().clearTextMessageDraft(conversationType,targetId);
```

### 自定义消息--message_customize--

以下示例采用 WebSDK 内置消息结构，自定义消息结构，请移步开发指南。

1、定义消息类型

```js
var messageName = "PersonMessage"; // 消息名称。
var objectName = "s:person"; // 消息内置名称，请按照此格式命名。
var mesasgeTag = new RongIMLib.MessageTag(true,true);// 消息是否保存是否计数，true true 保存且计数，false false 不保存不计数。
var propertys = ["name","age"]; // 消息类中的属性名。
RongIMClient.registerMessageType(messageName,objectName,mesasgeTag,propertys);
```

2、发送消息

```js
var conversationType = RongIMLib.ConversationType.PRIVATE; //单聊,其他会话选择相应的消息类型即可。
var targetId = "xxx"; // 想获取自己和谁的历史消息，targetId 赋值为对方的 Id。
var msg = new RongIMClient.RegisterMessage.PersonMessage({name:"zhang",age:12});
RongIMClient.getInstance().sendMessage(conversationType,targetId, msg, {
    onSuccess: function (message) {
    },
    onError: function (errorCode) {
    }
});
```

3、接收消息

接收消息与其他内置消息一致，在上文中提到的 ` setOnReceiveMessageListener ` 中增加 `case ` 将消息予以展示即可。

例如：` case RongIMClient.MessageType.RegisterMessage : dosomething...`

### 消息撤回--message_recall--

消息撤回操作服务器端没有撤回时间范围的限制，由客户端决定。

```js
//recallMessage  为需要撤回的消息对象
RongIMClient.getInstance().sendRecallMessage(recallMessage, {
        onSuccess: function (message) {
            showResult("撤回成功",message,start);
        },
        onError: function (errorCode,message) {
            showResult("撤回失败",message,start);
        }});
```

### 消息阅读回执--message_read_receipt--

单聊会话中消息阅读状态详细请参见[消息状态判定及处理方式](http://support.rongcloud.cn/kb/NjMz?_blank)。

### 清除服务端单群聊历史消息--clear_remote_history_messages--

<div class="bs-callout bs-callout-warning">
该功能需要在开发者后台“应用/IM 服务/高级功能设置”中开通公有云专业版后，开启`单群聊消息云存储`功能即可使用，如没有开启则执行 onError 方法。
</div>

```js
/**
  conversationType: 会话类型

  targetId: 目标 Id

  timestamp: 清除时间点，message.sentTime <= timestamp 的消息将被清除 (message: 收发实时或者历史消息中有 sentTime 属性)
  timestamp 取值范围:  timestamp >=0 并且 timestamp <= 当前会话最后一条消息的 sentTime
*/
var params = {
     conversationType: RongIMLib.ConversationType.PRIVATE, // 会话类型
     targetId: 'dPd90Fkja', // 目标 Id
     timestamp: 1513308018122 // 清除时间点
};
RongIMLib.RongIMClient.getInstance().clearRemoteHistoryMessages(params, {
  onSuccess: function() {
     // 清除成功
  },
  onError: function(error) {
      // 请排查：单群聊消息云存储是否开通
      console.log(error);
  }
});
```

## 会话接口--conversation--

### 会话类型--conversation_type--

1、`RongIMLib.ConversationType.PRIVATE` : 二人单聊会话类型，枚举值为 `1` 。

2、`RongIMLib.ConversationType.DISCUSSION` : 讨论组会话类型，枚举值为 `2` 。

3、`RongIMLib.ConversationType.GROUP` : 群组会话类型，枚举值为 `3`。

4、`RongIMLib.ConversationType.CHATROOM` : 聊天室会话类型，枚举值为 `4` 。

5、`RongIMLib.ConversationType.CUSTOMER_SERVICE` : 客服会话类型，枚举值为 `5` 。

6、`RongIMLib.ConversationType.SYSTEM` : 系统消息类型，枚举值为 `6` 。

7、`RongIMLib.ConversationType.APP_PUBLIC_SERVICE` : 公众账号（默认关注）会话类型，枚举值为 `7` 。

8、`RongIMLib.ConversationType.PUBLIC_SERVICE` : 公众账号 (手动关注) 会话类型，枚举值为 `8` 。

### 获取会话列表--conversation_list--

使用此方法需要在开发者后台“应用/IM 服务/高级功能设置”中开通公有云专业版后，开启`单群聊消息云存储`功能。

```js
RongIMClient.getInstance().getConversationList({
  onSuccess: function(list) {
    // list => 会话列表集合。
  },
  onError: function(error) {
     // do something...
  }
},null);
```

### 修改会话--conversation_edit--

此方法可以修改会话中信息，可以填充 `用户信息` 或会话 `Title` 等。

1、未使用本地存储，必须先执行 `getConversationList` 并且有这个会话此方法才有效。

```js
var conversationType = RongIMLib.ConversationType.PRIVATE;
var targetId = "xxx";
RongIMLib.RongIMClient.getInstance().getConversation(conversationType,targetId,{
        onSuccess:function(con){
            if (con) {
              con.conversationTitle="我是会话的Title";
              RongIMLib.RongIMClient.getInstance().updateConversation(con);
              consoleInfo("updateConversation Success!");
            }
        }
    });
```

2、使用本地存储

```js
var conversationType = RongIMLib.ConversationType.PRIVATE;
var targetId = "xxx";
RongIMLib.RongIMClient.getInstance().getConversation(conversationType,targetId,{
        onSuccess:function(con){
            if (con) {
              con.conversationTitle="我是会话的Title";
              RongIMLib.RongIMClient.getInstance().updateConversation(con);
              consoleInfo("updateConversation Success!");
            }
        }
    });
```

3、删除会话

```js
RongIMClient.getInstance().removeConversation(RongIMLib.ConversationType.PRIVATE,"1002",{
    onSuccess:function(bool){
       // 删除会话成功。
    },
	onError:function(error){
	   // error => 删除会话的错误码
	}
});
```

4、清除会话列表

`RongIMClient.getInstance().clearConversations(callback,[conversationTypes]);`

conversationTypes 为可选参数，可以清除指定会话类型的会话，可以多个。

5、清除所有会话：

```js
RongIMClient.getInstance().clearConversations({
    onSuccess:function(){
      // 清除会话成功
    },
    onError:function(error){
      // error => 清除会话错误码。
    }
});
```

6、清除指定会话：

```js
var conversationTypes = [RongIMLib.ConversationType.PRIVATE,RongIMLib.ConversationType.GROUP];
RongIMClient.getInstance().clearConversations({
    onSuccess:function(){
      // 清除会话成功
    },
    onError:function(error){
      // error => 清除会话错误码。
    }
},conversationTypes);
```

### 未读消息数--conversation_unread_message--

若浏览器不支持 `WebStorage` 并且您没有使用 `本地存储`，未读消息数将不会保存，浏览器页面刷新未读消息数将不会存在。

1、获取所有会话总未读消息数

```js
RongIMClient.getInstance().getTotalUnreadCount({
  onSuccess:function(count){
      // count => 所有会话总未读数。
  },
  onError:function(error){
      // error => 获取总未读数错误码。
  }
});
```

2、获取多个会话的未读消息总数

```js
var conversationTypes = [RongIMLib.ConversationType.PRIVATE,RongIMLib.ConversationType.DISCUSSION];
RongIMClient.getInstance().getConversationUnreadCount(conversationTypes,{
  onSuccess:function(count){
      // count => 多个会话的总未读数。
  },
  onError:function(error){
      // error => 获取多个会话未读数错误码。
  }
});
```

3、获取指定会话的未读消息数

```js
var conversationType = RongIMLib.ConversationType.PRIVATE;
var targetId = "xxx";
RongIMLib.RongIMClient.getInstance().getUnreadCount(conversationType,targetId,{
    onSuccess:function(count){
        // count => 指定会话的总未读数。
    },
    onError:function(){
        // error => 获取指定会话未读数错误码。
    }
});
```

4、清除未读消息数

```js
var conversationType = RongIMLib.ConversationType.PRIVATE;
var targetId = "xxx";
RongIMClient.getInstance().clearUnreadCount(conversationType,targetId,{
	onSuccess:function(){
		// 清除未读消息成功。
	},
	onError:function(error){
		// error => 清除未读消息数错误码。
	}
});
```

## 讨论组--discussion--

1、创建讨论组

```js
var discussionName = "讨论组名称"; // 名称自定义
var userIds = ["zhangsan","lisi"];//讨论组初始成员。
RongIMClient.getInstance().createDiscussion(discussionName,userIds,{
	onSuccess:function(discussionId){
		// discussionId => 讨论组 Id。
	},
	onError:function(error){
		// error => 创建讨论组失败错误码。
	}
});
```

2、讨论组加人

```js
var discussionId = "xxx"; // 讨论组 Id。
var userIds = ["wangwu","zhaoliu"];//被邀请成员。
RongIMClient.getInstance().addMemberToDiscussion(discussionId,userIds,{
	onSuccess:function(){
		// 邀请成功。
	},
	onError:function(error){
		// error => 邀请失败错误码。
	}
});
```

3、获取讨论组

```js
var discussionId = "xxx"; // 讨论组 Id。
RongIMClient.getInstance().getDiscussion(discussionId,{
	onSuccess:function(discussion){
		// discussion => 讨论组信息。
	},
	onError:function(error){
		// error => 获取讨论组失败错误码。
	}
});
```

4、退出讨论组

```js
var discussionId = "xxx"; // 讨论组 Id。
RongIMClient.getInstance().quitDiscussion(discussionId,{
	onSuccess:function(){
		// 退出讨论组成功。
	},
	onError:function(error){
		// error => 退出讨论组失败错误码。
	}
});
```

5、踢人

```js
var discussionId = "xxx"; // 讨论组 Id。
var userId = "xxxx"; // 将被移除讨论组的用户 Id。
RongIMClient.getInstance().removeMemberFromDiscussion(discussionId,userId,{
	onSuccess:function(){
		// 踢人成功。
	},
	onError:function(error){
		// error => 踢人失败错误码。
	}
});
```

6、设置讨论组邀请状态

```js
var discussionId = "xxx"; // 讨论组 Id。
var status = RongIMLib.DiscussionInviteStatus.OPENED; // 讨论邀请状态 0 ： 开发邀请，1：关闭邀请  RongIMLib.DiscussionInviteStatus.OPEND | RongIMLib.DiscussionInviteStatus.CLOSED。
RongIMClient.getInstance().setDiscussionInviteStatus(discussionId,status,{
	onSuccess:function(){
	},
	onError:function(error){
	}
});
```

7、设置讨论名称

```js
var discussionId = "xxx"; // 讨论组 Id。
var discusstionName = "新的讨论组名称";
RongIMClient.getInstance().setDiscussionName(discussionId,discusstionName,{
	onSuccess:function(){
	},
	onError:function(error){
	}
});
```

## 群组--group--

群组关系和群组列表由您的 App 维护，客户端的所有群组操作都需要请求您的 `App Server` ，
您的 `App Server` 可以根据自己的逻辑进行管理和控制，然后通过 `Server API` 接口进行群组操作，并将结果返回给客户端。

以下展示了客户端进行群组操作的流程。

** 创建群组 **

<canvas id="canvasSequenced_sendImageMessage" width="800" sequenced>
	App -> App Server: App 向自已应用服务器发起创建群组请求。
  App Server -> RongCloud Server: 授权成功后，在融云服务端同步创建群组。\n /group/create
	RongCloud Server -> App Server: 创建成功，返回状态。
	App Server -> App: 创建成功，可以发送群组信息。
</canvas>

** 加入群组 **

<canvas id="canvasSequenced_sendImageMessage" width="800" sequenced>
	App -> App Server: App 向自已应用服务器发起加入群组请求。
  App Server -> RongCloud Server: 授权成功后，调用融云服务端加入群组接口。\n /group/join
	RongCloud Server -> App Server: 加入成功，返回状态。
	App Server -> App: 加入成功，用户可在群组中发送信息。
</canvas>

** 退出群组 **

<canvas id="canvasSequenced_sendImageMessage" width="800" sequenced>
	App -> App Server: App 向自已应用服务器发起退出群组请求。
  App Server -> RongCloud Server: 授权成功后，调用融云服务端退出群组接口。\n /group/quit
	RongCloud Server -> App Server: 退出成功，返回状态。
	App Server -> App: 退出群组，用户不会再收到此群组信息。
</canvas>

** 解散群组 **

<canvas id="canvasSequenced_sendImageMessage" width="800" sequenced>
	App -> App Server: App 向自已应用服务器发起解散群组请求。
  App Server -> RongCloud Server: 授权成功后，调用融云服务端解散群组接口。\n /group/dismiss
	RongCloud Server -> App Server: 解散成功，返回状态。
	App Server -> App: 成功解散群组。
</canvas>

** 设置群组信息 **

<canvas id="canvasSequenced_sendImageMessage" width="800" sequenced>
	App -> App Server: App 向自已应用服务器发起设置群组信息请求。
  App Server -> RongCloud Server: 授权成功后，调用融云服务端设置群组信息接口。\n /group/refresh
	RongCloud Server -> App Server: 设置成功，返回状态。
	App Server -> App: 群组信息设置成功。
</canvas>


## 聊天室--chatroom--

1、加入

```js
var chatRoomId = "xxxx"; // 聊天室 Id。
var count = 10;// 拉取最近聊天最多 50 条。
RongIMClient.getInstance().joinChatRoom(chatRoomId, count, {
  onSuccess: function() {
   	// 加入聊天室成功。
  },
  onError: function(error) {
    // 加入聊天室失败
  }
});
```

2、退出

```js
var chatRoomId = "xxxx"; // 聊天室 Id。
RongIMClient.getInstance().quitChatRoom(chatRoomId, {
  onSuccess: function() {
   	// 退出聊天室成功。
  },
  onError: function(error) {
    // 退出聊天室失败。
  }
});
```

3、获取信息

```js
var chatRoomId = "xxxx";// 聊天室 Id。
var count = 10; // 获取聊天室人数 （范围 0-20 ）
var order = RongIMLib.GetChatRoomType.REVERSE;// 排序方式。
RongIMClient.getInstance().getChatRoomInfo(chatRoomId, count, order, {
  onSuccess: function(chatRoom) {
   	// chatRoom => 聊天室信息。
	// chatRoom.userInfos => 返回聊天室成员。
 	// chatRoom.userTotalNums => 当前聊天室总人数。
  },
  onError: function(error) {
    // 获取聊天室信息失败。
  }
});
```

4、设置获取云存储消息起始位置

```js
var chatRoomId = 'xxxx';
var timestamp = 0; // 获取历史消息起始时间（毫秒）
RongIMClient.getInstance().setChatroomHisMessageTimestamp(chatRoomId, timestamp);
```

5、获取云存储消息

该功能需开通后才能使用，详细请查看[聊天室消息云存储服务说明](/docs/payment.html#chatroom_message_cloud_storage?_blank)。

```js
var chatRoomId = 'xxxx';
var count = 10; // 拉取的条数 count <= 200
var order = 1;  // 1正序；0倒序
RongIMClient.getInstance().getChatRoomHistoryMessages(chatRoomId, count, order, {
  onSuccess: function(list, hasMore) {
    // list => 消息数组
    // hasMore => 是否有更多的历史消息
  },
  onError: function(error) {

  }
});
```

## 公众号--public_service--

融云公众服务包括：应用公众服务和公众服务平台，它为应用开发者和公众号运营者提供连接服务。

`应用公众服务`：是为应用开发者提供的 App 内建公众服务能力，在[融云开发者平台](https://developer.rongcloud.cn?_blank)应用公众服务中创建 App 公众号，实现应用内的公众服务。

`公众服务平台`：是在应用开发者和公众号运营者之间建立的对接平台，应用开发者可以通过平台引入公众服务资源，帮助 App 快速覆盖用户需求，公众号持有者通过平台可以有机会向所有集成融云 SDK 的 App 提供服务，进而获得更加精准更加丰富的受众渠道。[公众服务平台](http://public.rongcloud.cn?_blank)

### 创建、关联公众号--public_service_create--

可以在应用中关联公众服务平台创建的公众号，也可以创建应用内自有公众号。

1. 使用公众服务平台关联公众号

  ** 添加私有公众号 **

  在“开发者平台 -> 公众服务平台”中，添加私有公众号（需要应用申请上线后才能添加），输入公众号、识别码后关联成功。联系公众号提供者获取公众号及识别码：
  * 公众号：[公众服务平台](http://public.rongcloud.cn?_blank) -> 公众号设置 -> 公众号。
  * 识别码：[公众服务平台](http://public.rongcloud.cn?_blank) -> APP 接入模式设置 -> 私有模式 -> 识别码。

  ** 添加公有公众号 **

  在“开发者平台 -> 公众服务平台 -> 公众号市场”中，选择适合自已应用的公众号添加，公众号所有者审核通过后，与应用关联成功，可以应用中获取到相关信息。

2. 创建应用自有公众号

  在“[融云开发者平台](https://developer.rongcloud.cn?_blank) -> 应用公众服务”中创建自有公众号。创建成功后“公众号 Id” 为消息、会话功能中的 targetId。



## 黑名单

1、加入黑名单

```js
var userId = "xxxx";
RongIMClient.getInstance().addToBlacklist(userId,{
  onSuccess: function(chatRoom) {
   	// 加入黑名单成功。
  },
  onError: function(error) {
    // 加入黑名单失败。
  }
});
```

2、移除黑名单

```js
var userId = "xxx";
RongIMClient.getInstance().removeFromBlacklist(userId, {
  onSuccess: function() {
  },
  onError: function(error) {
  }
});
```

3、获取黑名单

```js
RongIMClient.getInstance().getBlacklist({
  onSuccess: function(list) {
   	// list => 黑名单列表
  },
  onError: function(error) {
    // 获取黑名单失败
  }
});
```

4、检查黑名单

```js
var userId = "xxx";
RongIMClient.getInstance().getBlacklistStatus(userId,{
  onSuccess: function(status) {
	// status => 状态
  },
  onError: function(error) {
  }
});
```

## 配置参数

### http 或 https

1、有服务的情况下无需设置 http 或 https，WebSDK 自动兼容 ，但访问静态文件必须，路径以 `file:///D:****` 开头的必须设置：

2、`DCloud` 、 `HBuilder` 开发者一定要设置。

`window["SCHEMETYPE"] = "http";`


## 其他资料

[融云官方开发者后台](https://developer.rongcloud.cn/signup?_blank)

[Web SDK API 文档](http://rongcloud.cn/docs/api/js/index.html?_blank)



> 融云小程序解决方案，咨询电话：13161856839

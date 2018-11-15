# IM 即时通讯解决方案

## 架构介绍

IM 即时通讯架构分为三部分: `RongCloud Server` 、`App Server`、`App (iOS、Android、Web、桌面版、小程序)`  交互如下:

![](http://rongcloud.cn/docs/assets/img/guide/archietecture.png)

### RongCloud Server

RongCloud Server 提供即时通讯及相关业务服务，按需或组合使用。

|服务	   	| 功能介绍
|:--------	| :--------------
|用户	   	| 详细查看：[获取 Token](http://rongcloud.cn/docs/server.html#user_get_token) 、[黑名单](http://rongcloud.cn/docs/server.html#black) 、[封禁](http://rongcloud.cn/docs/server.html#user_block) 功能介绍
|消息	   	| 详细请查看：[Android](http://rongcloud.cn/docs/android_imlib.html#message)、[iOS](http://rongcloud.cn/docs/ios_imlib.html#message)、[Web](http://rongcloud.cn/docs/web_api_demo.html#message) 、 [Server](http://rongcloud.cn/docs/server.html#open_source_sdk) 消息发送功能介绍
|会话		| 详细查看：[Android](http://rongcloud.cn/docs/android_imlib.html#conversation)、[iOS](http://rongcloud.cn/docs/ios_imlib.html#conversation_api)、[Web](http://rongcloud.cn/docs/web_api_demo.html#conversation) 会话功能介绍
|群组		| 详细查看：[群组](http://rongcloud.cn/docs/android_imlib.html#group)业务说明及 [Server API](http://rongcloud.cn/docs/server.html#group) 功能介绍
|聊天室		| 详细查看：[直播聊天室解决方案](http://rongcloud.cn/docs/live_chatroom.html)
|客服		| 详细查看：[客服服务开发指南](http://rongcloud.cn/docs/customer_service.html)
|音视频		| 详细查看：[音视频服务开发指南](http://rongcloud.cn/docs/call.html#open)
|公众号		| 详细查看：[公众服务开发指南](http://rongcloud.cn/docs/public_service.html)
|短信		| 详细查看：[短信服务开发指南](http://rongcloud.cn/docs/sms_service.html)
|消息推送		| 详细查看：[消息与推送说明文档](http://rongcloud.cn/docs/message_and_push_system.html)
|安全审核		| 详细查看：[安全审核产品](http://www.rongcloud.cn/product/antispam)及 [Server API](http://rongcloud.cn/docs/server.html#sensitiveword) 功能介绍

除以上基础服务，还提供`服务端实时消息路由`、`单群聊消息云存储`、`在线状态订阅`、`广播消息和推送`、`多设备消息同步`高级功能[查看详细](http://www.rongcloud.cn/docs/advanced_features.html)

### App Server

管理用户信息、好友关系、群组关系与 RongCloud Server 交互等，如果已有自己的 Server 可以参考 [Server 开发指南](http://rongcloud.cn/docs/server.html)

按需选择功能，也可参考融云开源 [SealTalk Server](https://github.com/sealtalk/sealtalk-server) 

### App

集成融云 SDK ，各端详解，请移步：[iOS](http://rongcloud.cn/docs/ios.html)、[Android](http://rongcloud.cn/docs/android.html)、[Web](http://rongcloud.cn/docs/web.html)、[桌面版](http://rongcloud.cn/docs/desktop.html)

`小程序` 请联系商务，咨询电话：`13161856839`

## SealTalk 

SealTalk 是使用融云即时通讯云服务及其 SDK 打造的开源即时通讯应用，覆盖主流平台，包括：iOS、Android、Windows、Mac、Web, 开发者可基于 SealTalk 打造自己的 IM App。

### 产品功能

|模块	   	| 功能
|:--------:	| --------------
| 账户管理	| 包括：登录、注册、找回密码、重置密码
| 消息发送	| 支持发送：文本、图片、语音、文件、动态表情、图文、群 @消息等内置消息及红包、名片等自定义消息类型
| 会话列表	| 按时间倒序显示所有聊天列表，提供置顶、删除会话、查看会话未读消息数等功能
| 联系人	    | 添加好友、好友列表、群组、关注公众号列表
| 公众号		| 搜索、关注、取消关注、查看公众号信息、已关注公众号列表
| 群组功能	| 创建、添加群成员、移除群成员、退出群组、解散群组、群成员列表、群信息等功能
| 音视频		| 单人音频、视频通话，多人音频、视频通话
| 个人设置	| 黑名单管理、新消息通知、推送设置

### 产品体验

应用下载，请移步 [下载页](http://www.rongcloud.cn/downloads)

### 快速集成

基于 SealTalk 打造应用，步骤如下：

![](http://7xogjk.com1.z0.glb.clouddn.com/graph.png)

** 步骤说明： **

1. 注册为融云开发者
	
	前往[融云官方网站](http://www.rongcloud.cn?_blank)注册创建融云开发者帐号。
	
2. 创建应用
	
	获取 App Key / Secret，它们是融云 SDK 连接服务器所必须的标识，每一个应用对应一套 App Key / Secret。针对开发者的生产环境和开发环境，提供两套 App Key / Secret，两套环境的功能完全一致。应用最终上线前，使用开发环境即可。
	
3. 下载源码	
	
	* [SealTalk Android 版源码](https://github.com/sealtalk/sealtalk-android?_blank)

	* [SealTalk iOS 版源码](https://github.com/sealtalk/sealtalk-ios?_blank)

	* [SealTalk Web 版源码](https://github.com/sealtalk/sealtalk-web?_blank)

	* [SealTalk 桌面版](https://github.com/sealtalk/sealtalk-desktop?_blank)

	* [SealTalk Server](https://github.com/sealtalk/sealtalk-server?_blank)
	
4. 集成调试
5. SealTalk Server 安装部署
	
	查看[配置安装与开发调试](https://github.com/sealtalk/sealtalk-server/blob/master/docs/install.md)说明文档。
	
6. 应用上线
	
	申请上线的应用，必须在融云开发环境下实现了发送消息测试。查看[上线注意事项](http://support.rongcloud.cn/kb/MzMw)


## 技术支持

[知识库](http://support.rongcloud.cn)

[工单](https://developer.rongcloud.cn/ticket)






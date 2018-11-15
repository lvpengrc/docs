# Server API 专有云功能

目前可提供的专有云功能：

* 发送聊天室广播消息
* 批量发送单聊模板消息
* 聊天室消息优先级服务
* 聊天室用户白名单服务
* 聊天室保活服务
* 聊天室全局禁言
* 聊天室消息白名单
* Server API 调用频率调整

## 发送聊天室广播消息方法

方法名：`/message/chatroom/broadcast`

## 批量发送单聊模板消息方法

方法名：`/senior/message/private/batch_template`

## 聊天室消息优先级服务

1、添加聊天室消息优先级方法

方法名：`/chatroom/message/priority/add`

2、移除聊天室消息优先级方法

方法名：`/chatroom/message/priority/remove`

3、查询聊天室消息优先级方法

方法名：`/chatroom/message/priority/query`

## 聊天室用户白名单服务

1、添加聊天室白名单成员方法

方法名：`/chatroom/user/whitelist/add`

2、移除聊天室白名单成员方法
方法名：`/chatroom/user/whitelist/remove`

3、查询聊天室白名单成员方法

方法名：`/chatroom/user/whitelist/query`

## 聊天室保活服务

1、添加保活聊天室方法

方法名：`/chatroom/keepalive/add`

2、移除保活聊天室方法

方法名：`/chatroom/keepalive/remove`

3、查询保活聊天室方法

方法名：`/chatroom/keepalive/query`

## 聊天室全局禁言

1、添加聊天室全局禁言方法

方法名：`/chatroom/user/ban/add`

2、移除聊天室全局禁言方法

方法名：`/chatroom/user/ban/remove`

3、查询聊天室全局禁言用户方法

方法名：`/chatroom/user/ban/query`

## 聊天室消息白名单

1、添加聊天室消息白名单

方法名：`/chatroom/whitelist/add`

2、移除聊天室消息白名单

方法名：`/chatroom/whitelist/delete`

3、查询聊天室消息白名单

方法名：`/chatroom/whitelist/query`
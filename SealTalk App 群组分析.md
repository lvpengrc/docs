群组模块分析
------
## 细分功能如下:
1.  群组创建
2.  群组邀请
3.  群组退出
4.  群组解散
5.  群组移除成员
6.  群组信息变更
7.  群组成员信息
8.  获取我的群组
9.  群组通知


>  1：下列所有接口的 Host 均为： http://api.sealtalk.im/
>  2:     以下所有接口调用请确保请求中已包含 Cookie
>  3： 示例数据中的状态码均为 200. 实际参考 [SealTalk Server](https://github.com/sealtalk/sealtalk-server)

---

## Group: Creator && Member
下面的用例图展示 群组 和 群组成员,对于某个群可执行的逻辑

![这里写图片描述](http://img.blog.csdn.net/20160901153831115)

> 1 . 此图展示仅为 SealTalk Group Creator && Member 逻辑展示 ，你的产品可根据产品需求做对应调整
> 2 .当前用户在群组中的权限 由 ***role*** 字段表示

---



## 移动端 群组表 & 群组成员表
Group table Column:

| groupId | name | portraitUri |displayName|role|timestamp|
|---

<br>
GroupMember table Column:

|groupId|userId|name|portraitUri|displayName|
|---


---




### 一:群组创建
接口: http://api.sealtalk.im/group/create

请求方式: post

请求参数: 

- name 群组名 ;
- memberIds 个数至少大于 1 的一组群组成员 id 数据；

返回数据: 

```
 示例数据:
 code : 200
 result : {"id":"ArVtlWJSv"}
```


> memberIds 从自己的好友列表中获取
> 创建群组的时候可选同时上传自己的群组头像(接口见下列群组信息变更)

![图片在联网状态下可见](http://img.blog.csdn.net/20160901151426730)

---

## 二:群组邀请

> 群组邀请 即是 群组中的成员已定的情况下另外邀请用户加入你的群组, 已在群组中的用户不允许重复邀请, 一次可以邀请一个或者多个用户。


接口: http://api.sealtalk.im/group/add

请求方式 : post

请求参数 : 

- groupId 群组 Id
- memberIds 个数至少大于 1 的一组群组成员 id 数据；

返回数据:

```
code : 200
```

![图片在联网状态下可见](http://img.blog.csdn.net/20160901153225841)

---

## 三:群组退出

接口: http://api.sealtalk.im/group/quit

请求方式 : post

请求参数 : 

- groupId 群组 Id

返回数据:

```
code : 200
```

![图片在联网状态下可见](http://img.blog.csdn.net/20160901154916275)

> quit 接口供群成员自行退出群组使用，另外建议在 IM 融云模块中 如果用户自行退出群组, 建议将本地 会话列表 的 item && cell 会话删除。

---

## 四:群组解散

接口: http://api.sealtalk.im/group/dismiss

请求方式 : post

请求参数 : 

- groupId 群组 Id

返回数据:

```
code : 200
```

> 1. 群组解散和上面退出的配图基本一致, 所以此处不展示配图
> 2. 群组解散后 群组成员将从服务端被退出该群组
> 3. 群组解散接口只有该群创建者可使用

---

## 五: 群组移除成员

接口: http://api.sealtalk.im/group/kick

请求方式 : post

请求参数 : 

- groupId 群组 Id
- memberIds 个数至少大于 1 的一组群组成员 id 数据；

返回数据:

```
code : 200
```

![图片在联网状态下可见](http://img.blog.csdn.net/20160901160850235)

> 1.被移除的 memberId 必须为群组成员 userId 
> 2.仅群组 创建者 有权限执行此操作

---

## 六:群组信息变更

- 群组名称变更
- 群组头像变更

### （1）群组名称变更

接口: http://api.sealtalk.im/group/rename

请求方式 : post

请求参数 : 

- groupId 群组 Id
- name     新的群组名称

返回数据:

```
code : 200
```

![这里写图片描述](http://img.blog.csdn.net/20160901162209368)

### (2)  群组头像变更


接口: http://api.sealtalk.im/group/set_portrait_uri

请求方式 : post

请求参数 : 

- groupId 群组 Id
- portraitUri     群组头像的 Url

返回数据:

```
code : 200
```

![这里写图片描述](http://img.blog.csdn.net/20160901164147635)

> SealTalk 的上传头像是存储 http:// url , 实际存储是在 七牛 云存储上。自己的产品可以自行选择图片存储方式

---

## 七:群组成员信息

> 获取群组成员信息有两种方式 : 1. 获取某个群的群组成员信息  2. 获取全群组的群组成员信息

### (1) 获取某个群的群组成员信息

接口: http://api.sealtalk.im/group/:groupId/members

请求方式 : get

请求参数 : 

- groupId 群组 Id

返回数据:

```
code : 200
result : [{"displayName":"","role":1,"createdAt":"2016-01-26T08:50:57.000Z","user":{"id":"6nx4DGtCu","nickname":"超时分辨率","portraitUri"......}]
```

> 此接口适用于点击群组详情页检查该群成员是否有数据变更, 如果有数据变更则更新 GroupMember 数据库

![这里写图片描述](http://img.blog.csdn.net/20160901170131476)

### (2) 获取全群组的群组成员


接口: http://api.sealtalk.im/user/sync/:version

请求方式 : get

请求参数 : 

- 无

返回数据:

```
  code : 200
  result : {"version":1234567894,"user":{"id":"sdf9sd0df98","nickname":"Tom","portraitUri":"http://test.com/user/abc123.jpg","timestamp":1234567891},"blacklist":[{"friendId":"sdf9sd0df98","status":true,"timestamp":1234567891}],"friends":[{"friendId":"sdf9sd0df98","displayName":"Jerry","status":20,"timestamp":1234567892}],"groups":[{"displayName":"Ironman","role":1,"isDeleted":true,"group":{"id":"sdf9sd0df98","name":"Team 1","portraitUri":"http://test.com/group/abc123.jpg","timestamp":1234567893}}],"group_members":[{"groupId":"cvx989vxc9","memberId":"sdf9sd0df98","displayName":"Ironman","role":1,"isDeleted":true,"timestamp":1234567893,"user":{"nickname":"Tom","portraitUri":"http://test.com/user/abc123.jpg"}}]}
```

> 此接口适用于登录同步数据 和 检查数据变更，数据内包含 个人数据，好友数据，黑名单数据，群组数据，群组成员数据。此处获取到群组成员数据可插入 GroupMember 供后续 UI 展示调用

---

## 八:获取我的群组

接口: http://api.sealtalk.im/user/groups

请求方式 : get

请求参数 : 

- 无

返回数据:

```
 code : 200
 result : [{"role":0,"group":{"id":"pG4lQsHkY","name":"我的群","portraitUri":"","creatorId":"7w0UxC8IB","memberCount":7}}]
```

> 此接口适用于登录的时候预加载群组数据至本地 Groups 数据库, 数据提供给 UI 界面展示调用。此接口可使用  七: 群组成员 当中的 sync 接口替代。

![这里写图片描述](http://img.blog.csdn.net/20160901170859858)

---

## 九: 群组通知

> 群组通知是群成员 或者 群创建者 对群做操作交互通知。群组中的成员皆会收到这个通知。以便做对应的 UI 展示 和 数据更新

目前 SealTalk 以 GroupNotificationMessage (ObjectName:RC:GrpNtf) 来做通知下发，当然你也可以自定义消息。此消息由 App server 下发到 客户端，SealTalk 被通知的情况有:

- add 邀请 或者 加入群组时
- quit 退出群组时
- kicked 被创建者移除群组时
- rename 群组更名时
- bulletin 群公告变更时
- create 群组创建时

![这里写图片描述](http://img.blog.csdn.net/20160901172148566)

---

## End:

### 上面已经将群组的全部 逻辑 以及 接口 和 数据库 以及使用方式做了详细介绍,相信你如果仔细阅读了上面文档 IM 群组模块对你来说应该没有问题(建议下载 [安装包](http://rongcloud.cn/sealtalk))边看文档边直接操作。







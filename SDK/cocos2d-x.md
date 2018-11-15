---
title: Cocos2d-x 游戏平台集成基础 IM 通讯能力开发指南 - 融云 RongCloud
description: 提供在 Cocos2d-x 应用中集成使用融云基础 IM 通讯能力的相关帮助和指南
template: doc.jade
---

# IMLib SDK for Cocos2d-x 开发指南

## Android 集成指南

### SDK 下载

[下载融云 SDK](http://www.rongcloud.cn/downloads?_blank)。

### 创建 Cocos2d-x 工程

命令行下输入：`cocos new -p <package name\> -l cpp <project name\>`，创建工程目录。
如：

```
cocos new -p rong.cocos.demo -l cpp CocosDemo
```

### Android Studio 打开工程

在 Android Studio 中选择 "Open an existing Android Studio project"。

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate01_1.png)
</div>

选择刚建好的 `Cocos` 工程下 `proj.android-studio` 目录，载入工程。

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate01_2.png)
</div>

### 集成 SDK

使用 `Project` 方式浏览：

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate02_1.png)
</div>

复制开发包中的 RongIMLib 目录到工程的 App 下：

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate02_2.png)
</div>

导入 `jar` 包，右键点击 `RongIMLib.jar` ，选择 `Add As Library...`：

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate02_3.png)
</div>

配置 `AndroidManifest.xml`

1. 添加权限：

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

2. 添加网络事件接收器：

```xml
<receiver android:name="io.rong.imlib.NetworkDealer$NetworkStatusReceiver">
    <intent-filter>
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
    </intent-filter>
</receiver>
```

### 集成推送功能

<div class="bs-callout bs-callout-info">
可选。如需要接收 push 消息，则按下面方式修改添加。
</div>

因为 Cocos 工程是模板生成的，`Activity` 的包名还用的是模板名，这里首先需要将其与工程包名修改一致。

1、修改 `package="org.cocos2dx.cpp"` 为工程真正的包名。（此例为 rong.cocos.demo，下同）

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate03_1.png)
</div>

2、右键单击 src 目录，新建 `rong.cocos.demo`，并将 `CocosdxActivity.java` 移动至其中。

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate03_2.png)
</div>

3、删除原有的 `org.cocos2dx.cpp` 包

添加推送事件接收器：

```xml
<receiver
    android:name="io.rong.push.notification.PushMessageReceiver"
    android:permission="rong.permission.push.com.rong.app">
    <intent-filter>
        <action android:name="io.rong.push.intent.action.message" />
    </intent-filter>
</receiver>

<service
    android:name="io.rong.push.PushService"
    android:process="io.rong.push">
    <intent-filter>
        <category android:name="android.intent.category.DEFAULT" />
        <action android:name="io.rong.push" />
    </intent-filter>
</service>

<receiver
    android:name="io.rong.push.PushReceiver"
    android:process="io.rong.push">
    <intent-filter>
        <action android:name="io.rong.push.intent.action.HeartBeat" />
    </intent-filter>
    <intent-filter>
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
    </intent-filter>
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
    <intent-filter>
        <action android:name="android.intent.action.USER_PRESENT"/>
        <action android:name="android.intent.action.ACTION_POWER_CONNECTED"/>
        <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED"/>
    </intent-filter>
</receiver>
```

在 `Activity` 标签中，添加通知栏消息事件接收器，注意包名要正确对应（此例为 rong.cocos.demo ）：

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:host="rong.cocos.demo"
          android:pathPrefix="/conversation"
          android:scheme="rong"/>
</intent-filter>
```

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate03_3.png)
</div>

**　修改 Android.mk 文件： **

加入静态库引用：

```java
include $(CLEAR_VARS)
LOCAL_MODULE    := librongimlib
LOCAL_SRC_FILES := ../RongIMLib/$(TARGET_ARCH_ABI)/librongimlib.a
include $(PREBUILT_STATIC_LIBRARY)

include $(CLEAR_VARS)
LOCAL_MODULE    := libsqlite
LOCAL_SRC_FILES := ../RongIMLib/$(TARGET_ARCH_ABI)/libsqlite.so
include $(PREBUILT_SHARED_LIBRARY)
```

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate04_1.png)
</div>

在原有的 `LOCAL_C_INCLUDES` 下加入 `$(LOCAL_PATH)/../RongIMLib/include` ，使其变为：

```java
LOCAL_C_INCLUDES := $(LOCAL_PATH)/../../../Classes \
                    $(LOCAL_PATH)/../RongIMLib/include
```

链接静态库：

```java
LOCAL_WHOLE_STATIC_LIBRARIES := librongimlib
LOCAL_SHARED_LIBRARIES := libsqlite
```

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate04_2.png)
</div>

**　修改 Application.mk 文件：　**

根据程序使用平台，添加对应支持，如：

```
APP_ABI := armeabi armeabi-v7a x86
```

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_android_Integrate05_1.png)
</div>


## iOS 集成指南

SDK 只支持 `xcode7.x`，不支持 `xcode6.x` ，支持 `iOS6.0` 以上版本。

### 集成 SDK

将压缩包解压后，找到 iOS 目录下的 `RongIMLib` 文件夹，拖到工程的 `FrameWorks` 下，如下所示：

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_ios_Integrate06.png)
</div>

在弹出的界面中选择如下：

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_ios_Integrate07.png)
</div>

同时请添加下图绿色框中的系统库：

<div style="border:solid 1px #999;width:800px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_ios_Integrate05.png)
</div>

<div class="bs-callout bs-callout-info">
注：iOS 10 下相关权限问题，请参考[适配 iOS 10 的注意事项](http://support.rongcloud.cn/kb/NTI5)。 HTTPS 相关设置请参考[ ATS 设置说明](http://support.rongcloud.cn/kb/NTU3)。
</div>

### 集成推送功能

如果游戏需要推送功能，请按照以下步骤添加推送支持。不需要可以略过。

打开 `AppController.mm` 文件，如下图位置

<div style="border:solid 1px #999;width:330px;margin:auto">
![image](/docs/assets/img/guide/cocos2d_ios_Integrate08.png)
</div>

导入头文件

```
#import "RongIM.h"
```

在 `application:didFinishLaunchingWithOptions:` 函数的最后，`return` 之前加上如下这段代码(推送处理 1 这段)

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {    

    ...

    /**
     * 推送处理1
     */
    if ([application
         respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        //注册推送, 用于 iOS8 以及 iOS8 之后的系统
        UIUserNotificationSettings *settings = [UIUserNotificationSettings
                                                settingsForTypes:(UIUserNotificationTypeBadge |
                                                                  UIUserNotificationTypeSound |
                                                                  UIUserNotificationTypeAlert)
                                                categories:nil];
        [application registerUserNotificationSettings:settings];
    } else {
        //注册推送，用于 iOS8 之前的系统
        UIRemoteNotificationType myTypes = UIRemoteNotificationTypeBadge |
        UIRemoteNotificationTypeAlert |
        UIRemoteNotificationTypeSound;
        [application registerForRemoteNotificationTypes:myTypes];
    }

    return YES;
}

```

另外在这个文件中加上如下两个函数

```
/**
 * 推送处理2
 */
//注册用户通知设置
- (void)application:(UIApplication *)application
didRegisterUserNotificationSettings:
(UIUserNotificationSettings *)notificationSettings {
    // register to receive notifications
    [application registerForRemoteNotifications];
}

/**
 * 推送处理3
 */
- (void)application:(UIApplication *)application
didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    NSString *token =
    [[[[deviceToken description] stringByReplacingOccurrencesOfString:@"<"
                                                           withString:@""]
      stringByReplacingOccurrencesOfString:@">"
      withString:@""]
     stringByReplacingOccurrencesOfString:@" "
     withString:@""];

    RongApi->setDeviceToken([token UTF8String]);
}
```

编译通过则集成工作就完成了。

## 使用说明

使用融云 SDK，需要导入头文件 `RongIM.h` ，并加入命名空间：

```objc
#include "RongIM.h"

USING_NS_RongIM;
```

异步回调通知主线程驱动，在 update 方法里写（只有安卓需要）：

```objc
RongApi->runInMainLoop();
```

### 连接及消息状态监听

主类继承连接状态监听和消息状态监听，并实现监听方法：

```
class HelloWorld : public cocos2d::Layer,
    public ConnectionStatusListener, // RongSDK 连接状态监听
    public MessageListener // RongSDK 消息状态监听
{
    ...
    // ConnectionStatusListener
    // 连接状态回调
    void onConnectionStatusChange(ConnectionStatus connectionStatus);

    // MessageListener
    // 发送进度回调
    void onSendProgress(const Message &sentMsg, int progress);
    // 发送成功
    void onSendSuccess(const Message &sentMsg);
    // 发送失败
    void onSendFailure(const Message &sentMsg, int code);
    // 接收消息回调
    void onReceivedMessage(const Message &rcvdMsg, int nLeft);
};
```

### 初始化 SDK

在程序入口，进行 SDK 初始化：

```objc
RongApi->setDebugLevel(DL_DEBUG); // 设置 Debug log 输出等级，可选
#if (CC_TARGET_PLATFORM == CC_PLATFORM_ANDROID)
    RongApi->setJVM(JniHelper::getJavaVM()); // 设置 JavaVM, 只有 Android 平台需要调用
#endif

    // 初始化 SDK，传入从开发者平台申请的应用 AppKey
    const char *appKey = "p5tvi9dst19k4";
    RongApi->init(appKey);

    // 注册连接状态监听和消息事件监听，需在 connect 之前完成
    RongApi->registerConnectionStatusListener(this);
    RongApi->registerMessageListener(this);

    // 以 "UserA" 的身份连接融云服务器，token 是 userId 对应的身份标识，可通过融云服务器提供的 RestAPI 获取到
    const char *token = "OBP6jghYYvQAmUi49wYbjS+XAKNTbHuH4vqlPeV7sUjc1p9lfcrXGw7EkVRGdfrHtF5Kji11dvbWS9GWr3YveA==";
    const char *userId = "UserA";
    RongApi->connect(token, userId);
```

### 断开连接

在程序退出的地方，加入 `disconnect` 方法：

```
void HelloWorld::menuCloseCallback(Ref* pSender)
{
    RongApi->disconnect(true); // RongSDK 断开连接方法
    Director::getInstance()->end();
#if (CC_TARGET_PLATFORM == CC_PLATFORM_IOS)
    exit(0);
#endif
}
```

### 会话与消息使用

融云 SDK 内置有数据库，存储了关于聊天的所有数据，可以调用 `RongApi` 来读取各种数据。

#### 1. 会话的操作

关于会话的接口如下：

```objc
/**
 *  获取会话列表。
 *
 *  会话列表按照时间从前往后排列，如果有置顶会话，则置顶会话在前。
 *
 *  @param conversationTypeList 会话类型数组，存储对象为NSNumber类型 type类型为int
 *  @return 会话列表。
 */
list<Conversation> getConversationList(const list<ConversationType> &conversationTypes) const;

/**
 *  从会话列表中移除某一会话，但是不删除会话内的消息。
 *
 *  如果此会话中有新的消息，该会话将重新在会话列表中显示，并显示最近的历史消息。
 *
 *  @param conversation 会话。
 *
 *  @return 是否移除成功。
 */
bool removeConversation(const Conversation &conversation) const;

/**
 *  设置某一会话为置顶或者取消置顶。
 *
 *  @param conversation 会话。
 *  @param isTop            是否置顶。
 *
 *  @return 是否设置成功。
 */
bool setConversationToTop(const Conversation &conversation, bool isTop) const;

/**
 *  获取来自某用户（某会话）的未读消息数。
 *
 *  @param conversation 会话。
 *
 *  @return 未读消息数。
 */
int getUnreadCount(const Conversation &conversation) const;

/**
 *  获取某会话类型的未读消息数.
 *
 *  @param conversationTypes 会话类型, 存储对象为NSNumber type类型为int
 *
 *  @return 未读消息数。
 */
int getUnreadCount(const list<ConversationType> &conversationTypes) const;

```

<div class="bs-callout bs-callout-warning">
对于普通的聊天应用，是有会话概念的，需要显示所有的会话记录。但对于游戏可能就不需要聊天列表，只需要一个世界频道（公共聊天室）、一个公会频道（群组），和私聊频道。游戏需要记住各个会话的类型和 id。
<br>
`removeConversation` 仅清除数据库中的会话记录，会话内的消息都还存在。如果想彻底删除，可以先调用 `clearMessages`，然后再调用 `removeConversation` 。
</div>

#### 2. 消息列表的读取

消息列表的读取有如下两个函数，消息需要分批拉取，第一次要调用 `getLatestedMessages` ，返回 `count` 数目的消息。之后调用 `getHistoryMessages` ，参数 `oldestMessageId` 传入上次拉取最后一条消息的 `messageId` 。消息拉取的顺序是最新的消息在位置 `0`，最旧的消息在最后。

```

/**
 *  获取最新消息记录。
 *
 *  @param conversation 会话。
 *  @param count            要获取的消息数量。
 *
 *  @return 最新消息记录，按照时间顺序从新到旧排列。
 */
list<Message> getLatestedMessages(const Conversation &conversation, unsigned int count) const;

/**
 *  获取历史消息记录。
 *
 *  @param conversation 会话。
 *  @param oldestMessageId  最后一条消息的 Id，获取此消息之前的 count 条消息。
 *  @param count            要获取的消息数量。
 *
 *  @return 历史消息记录，按照时间顺序新到旧排列。
 */
list<Message> getHistoryMessages(const Conversation &conversation, long oldestMessageId, unsigned int count) const;
```

<div class="bs-callout bs-callout-warning">
游戏的私聊可能都在一个私聊频道内，因此需要获取所有的单聊会话的所有消息。此时会话的会话类型设置为单聊类型，`targetId` 设置为空字符串即可。
</div>

#### 3. 消息的操作

对消息的数据库操作如下：

```
/**
 *  插入一条消息。
 *
 *  @param conversation 会话。不支持聊天室会话。
 *  @param senderUserId     消息的发送者，如果为空则为当前用户。
 *  @param sendStatus       要插入的消息状态。
 *  @param content          消息内容
 *
 *  @return 插入的消息实体。
 */
Message insertMessage(const Conversation &conversation, const string &sendUserId, MessageStatus sendStatus, MessageContent *content) const;

/**
 *  删除指定的一条或者一组消息。
 *
 *  @param messageIds 要删除的消息 Id 列表, 存储对象为NSNumber
 *messageId，类型为long。
 *
 *  @return 是否删除成功。
 */
bool deleteMessages(const list<long> &msgIds) const;

/**
 *  清空某一会话的所有聊天消息记录。
 *
 *  @param conversation 会话。不支持聊天室会话。
 *
 *  @return 是否清空成功。
 */
bool clearMessages(const Conversation &conversation) const;

/**
 *  清除消息未读状态。
 *
 *  @param conversation 会话。不支持聊天室会话。
 *
 *  @return 是否清空成功。
 */
bool clearMessagesUnreadStatus(const Conversation &conversation) const;

/**
 *  清空会话列表
 *
 *  @param conversationTypeList 会话类型列表，存储对象为NSNumber, type类型为int
 *
 *  @return 操作结果
 */
bool clearConversations(const list<ConversationType> &conversationTypes) const;

/**
 *  设置接收到的消息状态。
 *
 *  @param messageId      消息 Id。
 *  @param receivedStatus 接收到的消息状态。
 */
bool setMessageReceivedStatus(long messageId, MessageStatus receivedStatus) const;


/**
 *  设置消息发送状态。
 *
 *  @param messageId      消息 Id。
 *  @param sentStatus 接收到的消息状态。
 */
bool setMessageSentStatus(long messageId, MessageStatus sentStatus) const;

```

<div class="bs-callout bs-callout-warning">
发送消息时，SDK 会自动 `insertMessage`（MessagePsersistent 为存储类型）。
<br>
`clearMessages` 后，只删除了会话内的消息，会话记录还存在与数据库中。当需要清理多个会话内的消息时调用 `clearConversations`。
<br>
`setMessageSentStatus` 。发送的消息会自动更新 `sentstatus` ，不用干预。
</div>

#### 4. 消息内容的使用

消息内容类是多态的，它的共同基类是 `MessageContent` 。当有一个 `Message` 对象时，首先要判断消息内容类是哪一种类型，然后做强制转换后进行处理。

```
void HelloWorld::onReceivedMessage(const Message &rcvdMsg, int nLeft) {
    if (rcvdMsg.m_content->isText()) {
        TextMessageContent *textContent = (TextMessageContent *)rcvdMsg.m_content;
        CCLOG("received text content is %s", textContent->m_content.c_str());
    } else if (rcvdMsg.m_content->isAudio()) {
        AudioMessageContent *audioContent = (AudioMessageContent *)rcvdMsg.m_content;
        CCLOG("received audio content, audio url is %s, duration is %ld", audioContent->m_mediaUrl.c_str(), audioContent->m_duration);
    } else if(其它内置消息类型) {
        //其它内置消息类型的处理
    } else if(rcvdMsg.m_content->getObjectName() == "XX:XXXXMessage") {
        XXXXMessageContent *customMsg = (XXXXMessageContent *)rcvdMsg.m_content;
        //自定义消息的处理
    }
}  
```

#### 5. 媒体消息内容

媒体消息内容的基类是 `MediaMessageContent` ，包含有 `ImageMessageContent` 和 `AudioMessageContent` 两个子类。这类消息与其它消息类型的处理方式是不同的：

* 发送时：先上传媒体文件到媒体文件服务器，然后发送包含链接的消息。（上传和发送是 SDK 自动处理的，跟普通消息一样，调用 `sendMessage` 接口就行）
* 接收时：收到的是包含链接的消息，当使用时调用 `RongApi->downloadMedia` 来下载媒体文件
* 发送语音消息时，请使用 `RongApi->startRecordVoice` 来录音。接收到语音消息时，使用 `RongApi->downloadMedia` 来下载媒体文件，Android 平台为 amr 格式，iOS 平台为 wav 格式，播放还请调用各自平台方法自行处理。发送和接收下载的过程中有涉及到声音编解码的工作，请务必确保使用 SDK 的方法。图片或着其它类型媒体文件也可以使用其它方法下载。

发送文本消息：

```java
Conversation conversation(ConversationType_PRIVATE, "UserB");
MessageContent *textContent = TextMessageContent::createContent("Hello world");
RongApi->sendMessage(conversation, textContent);
```

发送图片消息：

```java
Conversation conversation(ConversationType_PRIVATE, _targetId);
  string path = FileUtils::getInstance()->fullPathForFilename("HelloWorld.png");
  MessageContent *imageContent = ImageMessageContent::createContent(path);
RongApi->sendMessage(conversation, imageContent);
```

发送语音消息：

```java
Conversation conversation(ConversationType_PRIVATE, _targetId);
  MediaMessageContent *voiceContent = AudioMessageContent::createContent(voicePath, 			duration);
RongApi->sendMessage(conversation, voiceContent);
```

#### 6. 发送和接收消息时，消息列表的建议

在消息界面生成时，读取一次数据库，把需要显示的消息存放到列表中。之后每当收到消息或者发送消息时，把新消息加入到列表的最前端。当用户下拉获取更多历史消息时（`getHistoryMessages`），读取的消息放倒列表的尾部。然后把列表中的内容显示在界面上。

### 自定义消息

SDK 内置的消息内容类不足以满足用户多样的需要，因此提供了自定义消息的方法来扩展消息，满足用户的各种需求。

#### 1. 使用现有模版

* SDK 内置了四个消息内容类 `CommandMessageContent`/`NotificationMessageContent`/`CustomMessageContent`/`StatusMessageContent` ，参数有 `type` 、 `content` 还有所有消息都有的 `extra` 字段。用户可以把消息内容放入到这几个字断中用来传递消息。比如 type 中放入消息的类型，`content` 和 `extra` 中放入消息的内容。如果内容字段过多，可以放到一个 `json` 串中。
* 这四种消息不同之处在于本地数据库存储和服务器处理。`CommandMessageContent` 消息内容在本地不存储不计数，`NotificationMessageContent` 消息内容只存储不计数，`CustomMessageContent` 消息内容存储并计数，`StatusMessageContent` 消息内容不存储不计数，且如果对端不在线就把消息直接抛弃。
* 当用户有多种消息使用同一种模版时，用户需要来做好区分工作。

#### 2. 自定义消息内容

除了使用 `1` 的模版外，还可以自定义消息内容类。自定义消息内容类需要做到如下几个方面。

##### 1. 定义自定义消息内容类的工厂类

实现以下 `3` 个虚函数。

```
class MyContentFactory : public MessageContentFactory {
public:
    virtual const string getObjectName() const
    {
        return "MC:Content";
    }
    virtual MessagePsersistent getPersistentFlag() const {
        return MessagePersistent_PERSIST_COUNT;
    }
    virtual MessageContent* createEmptyContent() const
    {
        return new MyContent();
    }
};
```

<div class="bs-callout bs-callout-warning">
`ObjectName` 是在网络上传输的名称，必须确保唯一性。融云内置的消息都是以 `RC：` 开头，建议 App 的自定义消息以自己公司缩写开头避免重复。
</div>

##### 2. 定义自定义消息内容类

定义自定义消息内容类需要实现以下 `4` 个虚函数。

```
class MyContent : public MessageContent {
    public:
        virtual string encode() const;
        virtual void decode(const string &rawData);
        virtual const MessageContentFactory* getFactory() const ;
        MessageContent* copy() const;

        //类的其它成员
    };
```

<div class="bs-callout bs-callout-warning">
`encode` 用来消息内容编码，把消息内容转化为可读的字符串。内置的消息都是使用 `json` 序列化各个成员，非字符串成员可以用 `base64` 处理一下。
<br> `decode` 用来消息内容解码，把字符串转化为消息内容。必须确保经过 `encode` 编码出来的字符串，再调用 `decode` 能够得到一样的消息内容。
<br>
`getFactory` 获取消息内容工厂类，返回一个 1 定义的对象
<br>
`copy` 函数提供消息内容深 `copy` 的能力。
</div>

##### 3. 注册自定义消息内容工厂类

建议在调用完 `init` 之后注册。

```
const MyContentFactory ＊factory = new MyContentFactory();
RongApi->registerMessageContentFactory(factory);
```

##### 4. 自定义消息内容类的使用

做到以上 `3` 步，你的自定义消息内容类就与内置的消息内容类一样了，你可以通过以下代码发送。接收方在 `MessageListener` 中就会收到该消息

```
MyContent *myContent = new MyContent();
myContent.xxx = ...;
RongIM::Message message = RongApi->sendMessage(Conversation(RongIM::ConversationType_PRIVATE, "receiverId"), myContent);
```

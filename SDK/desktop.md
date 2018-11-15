---
title: 桌面版解决方案
description: 桌面版解决方案
template: doc.jade
---
# 桌面版解决方案

##  架构介绍--framework--

1、支持 macOS、Windows、Linux

2、架构介绍

​        桌面版基于 [Electron](https://electronjs.org/) 开发。应用分为主进程和渲染进程。主进程通过 Node.js 的 API 跨过网页运行的沙箱实现跟操作系统底层交互。渲染进程基于 Chrominum(类 Chrome 的浏览器) 渲染界面。

3、目的

通过 HTML, CSS, 和 JavaScript 技术建立跨平台 (Mac，Windows和Linux) 的桌面版应用。

##  功能模块--modules--

| 模块名称       | 功能简介                | 说明                                       |
| ---------- | :------------------ | :--------------------------------------- |
| IM SDK     | 与融云服务器通讯            | c++ 开发，编译成 [Node 原生模块](https://electronjs.org/docs/tutorial/using-native-node-modules) 后由 js 调用 |
| Download   | 文件下载                |                                          |
| File       | 文件操作，如 打开指定文件 等等    |          |
| Screenshot | 获取用户截屏(可编辑)的图片   | c++ 开发，编译成 [Node 原生模块](https://electronjs.org/docs/tutorial/using-native-node-modules) 后由 js 调用 |
| System     | 系统操作，如清除缓存，获取当前平台 等 |                                          |
| Window     | 窗口操作，如 最大化，最小化等等    |                                          |
| Upgrade    | 在线热更新               | 仅更新需要升级的模块                               |

##  API 文档

### 文件下载--download--

文件下载仅支持 http(s) 协议。

#### 构造函数

名称：download

参数：file、config

- file：

    - `url`：下载地址

    - `name`：文件保存后的别名

- config：

    - `timeout`：连接超时，默认 0

    - `retries`：出错时重试次数，默认 10

#### 方法

1、保存方法，默认保存到 Download 目录。

* 名称：save

* 参数：name

  `name`：下载别名，默认为原文件名

2、另存为，以指定名称和指定路径

* 名称：saveas

* 参数：file

  file 描述：

  `name`：另存为对话框默认文件名称

  `path`：另存为对话框默认路径

3、暂停下载

* 名称：pause

* 参数：`无`

4、恢复下载

* 名称：resume

* 参数：`无`

5、取消下载

* 名称：abort

* 参数：`无`

### 事件

1、下载链接初次响应事件，可获得总文件大小

* 名称：onReady

* 参数：`无`

* 回调参数：file

- file 描述：

    - `total`：文件大小

    - `path`：保存路径

2、下载中

- 名称：onProgress

- 参数：`无`

- 回调参数：`file`

- file 描述：

    - `loaded`：已下载大小

    - `total`：文件大小

    - `path`：保存路径

3、下载完成；取消下载也会触发此方法。

- 名称：onComplete

- 参数：`无`

- 回调参数：`file`

- file 描述：

    - `loaded`：已下载大小

    - `total`：文件大小

    - `path`：保存路径

** onTimeout **

回调参数：error

- error：

    - `code`： "ETIMEDOUT"

    - `connect`：true/false。超时分`连接超时`和`读取超时`。connect 为 true 时为`连接超时`，false 为`读取超时`详见 [Timeouts](https://www.npmjs.com/package/request?_blank)

    - `message`："ETIMEDOUT"

    - `stack`：错误堆栈

4、下载超时

参数：`无`

** onError **

回调参数：error

- error：

    - `code`：错误码

    - `message`：错误信息

    - `stack`：错误堆栈

    - `description`：错误可能的原因, 参照下述常见错误码描述

5、下载异常

参数：`无`

常见错误码

- `ENOTFOUND`：dns 找不到域名导致。请检查 1. 域名是否写错 2. 网络连接是否正常

- `ECONNREFUSED`：这可能是因为防火墙或代理程序无法访问网络。请检查防火墙或者代理设置

- `ECONNRESET`：options 加入 {originalHostHeaderName: 'Host'}。参考 [https://stackoverflow.com/questions/38262438/nodejs-request-throw-error-read-econnreset-when-make-http-call]

- `EPIPE`：低版本 nodejs 易发生此错误

- `ETIMEDOUT`：连接超时

### 文件操作--Files--

1、检查文件是否存在

方法：checkExist

参数：path

- `path`：文件路径

返回值：`exist`

- `exist`：存在为 true，反之 false

2、获取粘贴板的文件(夹)，支持多个文件/文件夹

方法：getFiles

参数：`无`

返回值：`Object`

- `Object`: {"dirList": [{path, size}], "fileList": [{lastModified, name, localPath, size, type}]}, {文件夹数组, 文件数组}

3、打开指定文件

方法：open

参数：path

- `path`：文件路径

4、打开指定文件所在文件夹，并选中文件

方法：openDir

参数：path

- `path`：文件夹路径

### 载图--Screenshot--

#### 方法

** 1、start **

描述：截图方法

参数：(callback(file))

- file：截屏返回数据

    - `dataURL`：dataURL

    - `base64`：base64 字符串

    - `size`：大小

    - `mime`：mime 类型

** 2、setShortcut **

描述：设置截图快捷键

参数：(shortcut)

- `shortcut`：快捷键字符，为空则取消快捷键；Mac：'Command+Ctrl' + shortcut；Windows：'CTRL+ALT' + shortcut。例如 如果目标快捷键为 'Command+Ctrl+S'，则 shortcut 值为 'S'

#### 注册事件

** onComplete **

描述：截屏完成后事件；如果调用截屏无法设置回调时，比如通过菜单调用截屏，页面需注册该事件以接收截屏返回数据

参数：(file)

- file：截屏返回数据

    - `dataURL`：dataURL

    - `base64`：base64 字符串

    - `size`：大小

    - `mime`：mime 类型

### 系统--system--

#### 方法

* 名称：clearCache

  清除缓存，包括 HTTP cache、appcache、cookies、filesystem、indexdb、localstorage、shadercache、websql、serviceworkers；

  参考 [session.clearCache， session.clearStorageData](https://electronjs.org/docs/api/session?_blank) 清除成功后跳转到主页

* 参数：`无`

#### 属性

* deviceId：设备id

  返回值：`String`，MAC 地址

* locale：系统语言设置

  返回值：`String`，参考 [https://electronjs.org/docs/api/locales](https://electronjs.org/docs/api/locales?_blank)

* platform：当前平台

  返回值：`String`，标识 Node.js 进程运行其上的操作系统平台。 例如：'darwin'、'freebsd'、'linux'、'sunos' 或 'win32'

### 窗口--Window--

1、关闭窗口

方法：close

参数：`无`

2、获取焦点

方法：focus

参数：`无`

3、最大化

方法：max

参数：`无`

4、最小化

方法：min

参数：`无`

5、取消最大化/最小化

方法：restore

参数：`无`

**  **

6、前置/取消前置窗口；设置窗体是否显示于其它窗体之上

方法：setAlwaysOnTop

参数：isOnTop

- `isOnTop`：是否前置窗口

7、振动窗体

方法：shake

参数：interval， time

- `interval`：振动频率

- `time`：振动时间

缺省值：`(25, 1000)`

8、显示

方法：show

参数：`无`

9、显示但不激活

方法：showInactive

参数：`无`

##  开发环境

1、运行环境: [Nodejs 5.3.0](https://nodejs.org/dist/latest-v5.x/docs/api/)。

2、开发工具:  [Electron 1.4.15](https://github.com/electron/electron/tree/v1.4.15/docs)。 [Electron](https://electronjs.org/) 基于 Chromium 和 Node.js，使用 HTML，CSS 和 JavaScript 构建应用。

3、打包工具: [electron-packager](https://www.npmjs.com/package/electron-packager) 是一个基于 Node.js 的命令行工具，它将基于 Electron 的应用源码打包成可执行程序，如 app(Mac)，exe(Windows) 等，并将打包后的可执行文件和相关支持文件放入待分发的文件夹中。

4、安装包工具: 

- Mac: [electron-builder](https://www.npmjs.com/package/electron-builder) 用于建立一个可分发的 Electron 应用。

- Windows: [Inno Setup](http://www.jrsoftware.org/isinfo.php) 用于建立 Windows 平台的分发应用。

##  安装包制作

- 打包: 

    - macOS: 

    ```
    npm run package:mac
    ```

    - Windows: 

    ```
    npm run package:win
    ```

- 制作安装包: 

    - macOS: 

    ```
    npm run installer:mac
    ```

    - Windows: 打开项目文件 \*.iss, 选择菜单 Build/Compile

##  成品体验

[SealTalk Windows](http://downloads.rongcloud.cn/SealTalk_by_RongCloud_1_0_2.exe)

[SealTalk Mac](http://downloads.rongcloud.cn/SealTalk_by_RongCloud_1_0_2.dmg)

##  其他参考资源

- nodejs 文档

    - [官方 api](https://nodejs.org/dist/v5.3.0/docs/api/)

- Electron 

    - [官方文档](https://electronjs.org/docs)

    - [官方入门 demo](https://github.com/electron/electron-quick-start)

    - [官方 api demo](https://github.com/electron/electron-api-demos)

    - [Electron 与 NW.js 的技术差异](https://electronjs.org/docs/development/atom-shell-vs-node-webkit)

- NW.js

    - [官方文档](http://docs.nwjs.io/en/latest/)






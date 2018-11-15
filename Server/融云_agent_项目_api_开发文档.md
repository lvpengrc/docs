# 融云 Agent 项目 API 开发文档

## 功能目录

一级         |二级
-------------|:------------
API 校验规则  |API 接口签校验规则

## 一、API 接口签校验规则

所有请求 API 接口均使用此规则校验，后面不再复述此规则：

每次请求 API 接口时，均需要提供 4 个 HTTP Request Header：* App-Key:（融云分配的App-Key）* Nonce：（随机数）* Timestamp：（时间戳）* Signature:（数据签名）	** 签名代码示例 - PHP **
```$timestamp = time(); //获取时间戳$nonce = rand();//获取一个随机数$appSecret = 'YlW2MeFwwRx';//系统分配的密码$signature = sha1($appSecret.$nonce.$timestamp);```
## 二、应用功能接口
### 1、创建应用
URL 地址：http://api.developer.rongcloud.cn/app/createApp.json
方法：POST** POST 参数： **

参数	    |类型     |说明
:--------|:---------|:----------name	|String    |应用名称，长度为 255 个字符 category |Int      |应用类别，长度为 2 个字符（类别详细参考底部文档）description	|String   |应用描述，长度为，1000 个字符。 userId	|Int       |用户id**  返回数据格式：JSON **```
{"code":"2000","data":{"name":"\u5ba2\u670d\u6d4b\u8bd5\u8d26\u53f7","category":"\u53c2\u8003","production_app_key":"0vnjpoadnz0oz","dev_app_key":"pgyu6atqydpau","production_app_secret":"MCY6m1JXiuLyYV","dev_app_secret":"qPlSLgSthJuF","production_certificate":"","dev_certificate":"","create_time":1416307190,"service_status":"\u5f00\u542f","status":"\u672a\u7533\u8bf7"}}```
** 状态码说明： **
code	|说明
--------|:--------5001	|应用创建错误3004	|应用名称重复3005	|应用保存错误** 字段说明： **字段	      |说明
----------|:----------name	  |应用名称category  |应用类别production_app_key	    |生产环境 App Keydev_app_key	|开发环境 App Keyproduction_app_secret	|生产环境 App Secretdev_app_secret	|开发环境 App Secretproduction_certificate	|生产环境证书dev_certificate	|开发环境证书create_time	    |应用创建时间service_status	|服务状态status	   |应用状态### 2、修改应用信息
URL 地址：http://api.developer.rongcloud.cn/app/editApp.json
方法：POST** POST 参数： **参数	     |类型       |说明
:---------|:---------|:---------name	|String    |应用名称，长度为 255               category	|Int    |应用类别，长度为 2 ，（参考底部文档）description	|String   |应用描述，长度为 1000              new_name	|String   |新应用名称，长度为 255        userId	|Int     |用户id
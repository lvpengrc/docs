# Token 绑定设备(DeviceId)登录流程梳理

## 功能说明

Token 绑定设备后，该 Token 只能在绑定的设备上连接融云服务器，加强了 Token 使用的安全性。

## 绑定设备连接融云流程

1. 获取设备 deviceId
2. 通过 deviceId 获取 Token
3. 使用 Token 连接融云服务器

## 获取设备 deviceId

### Android 端

SDK 中提供获取 deviceId 接口：

```
DeviceUtils.getDeviceId(Context context, String appKey)
```

可通过以上接口获取 deviceId 后，再通过 App Server 调用融云 Server API 接口获取 Token。

### iOS 端

SDK 中不提供获取 deviceId 接口，开发者需要自已获取 deviceId 后，再通过 App Server 调用融云 Server API 接口获取 Token。


### SDK 错误处理

客户端连接融云时，融云 Server 对 Token 进行验证，如当前申请连接的设备与 Token 中绑定的 deviceId 不符，则返回 31021（RC_CONN_DEVICE_ERROR）错误码。

开发者可根据错误码，做相关的业务处理。


## 通过 deviceId 获取 Token

融云获取 Token 接口中传入 deviceId 获取的 Token，只能在 deviceId 对应的设备上使用。示例：

```
POST /user/getToken.json HTTP/1.1
Host: api.cn.ronghub.com
App-Key: uwd1c0sxdlx2
Nonce: 14314
Timestamp: 1408706337
Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
Content-Type: application/x-www-form-urlencoded

userId=jlk456j5&name=Ironman&portraitUri=http%3A%2F%2Fabc.com%2Fmyportrait.jpg&deviceId=wCnvhuiwlwZO2YmfCKe08g
```

** deviceId 参数说明 **：String 类型，非必填项，携带此参数获取的 Token 即只能在指定设备上使用




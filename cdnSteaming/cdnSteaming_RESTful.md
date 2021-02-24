# 旁路推流 RESTful 调用方法

使用 RESTful API，无需集成 SDK，通过自己的业务服务器，可以向URTC服务请求 开启/关闭、更新 旁路推流的参数。

 - 开始/关闭 旁路推流
 - 查询当前的 旁路推流
 - 更新 旁路推流的流内容
 - 更新 合流配置


## 1. URTC RESTful API数据格式

### 1.1 请求中的公共字段

```json
{
    "Version": "1.0",
    "Action":"xxx",
    "": "",
    "Internal": {
        ...
    },
    "Data": {
        ...
    }
}
```

接口 | 描述
--- | ---
Version| 服务版本，如果后台服务版本升级，可通过此字段完成向前兼容，当前服务版本`1.0`。
Action| 请求类型，详情参看上文 [2.1 接口列表](urtc/cdnSteaming/cdnSteaming_RESTfulAPI?id=_21-接口列表)。
Token| 鉴权Token，生成规则参考 [Token生成指导](urtc/sdk/token)。
Internal| 不同Action需要携带的与频道、房间等配置有关的参数。
Data| 不同Action需要携带的跟转码、合流、转推等配置有关的参数。


## 1.2 返回中的公共字段D

```json
{
    "Version": "1.0",
    "Ack": "xxx",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        ...
    },
    "Data": {
        ...
    }
}
```

接口 | 参数类型 | 描述
--- | ---| ---
Version|string类型|同请求字段中的`Version`。
Ack| string类型|详情参看上文 [2.1 接口列表](urtc/cdnSteaming/cdnSteaming_RESTfulAPI?id=_21-接口列表)。
RetCode|int类型|错误代码，0 成功，非零代表失败，具体错误代码请参考错误代码总结。
Message|string类型|错误的文本提示。
Internal|json对象|不同Action需要携带的与频道、房间等配置有关的参数。
Data|json对象|根据不同的请求类型，data中的内容也不同，其中包含着具体请求结果的私有数据。

## 2. 旁路推流的调用时序

下图为实现 旁路推流 需要调用的 API 时序图。    

[](images/cdnSteamingImage/cdnSteamingStart.png）

>查询、更新流、更新合流配置都是可选的，且可以多次调用，但是必须在旁路推流过程中（开始旁路推流后到结束旁路推流前）调用。


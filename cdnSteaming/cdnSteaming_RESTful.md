# 旁路推流 RESTful 调用方法

使用 RESTful API，无需集成 SDK，通过自己的业务服务器，可以向URTC服务请求 开启/关闭、更新 旁路推流的参数。

 - 开始/关闭 旁路推流
 - 查询当前的 旁路推流
 - 更新 旁路推流的流内容
 - 更新 合流配置


## 1. URTC RESTful API数据格式

### 1.1 请求的公共字段

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
Action| 请求类型，详情参看RESTful API [2.1 接口列表](urtc/cdnSteaming/cdnSteaming_RESTfulAPI?id=_21-接口列表)。
Token| 鉴权Token，生成规则参考 [Token生成指导](urtc/sdk/token)。
Internal| 不同Action需要携带的与频道、房间等配置有关的参数。
Data| 不同Action需要携带的跟转码、合流、转推等配置有关的参数。


## 1.2 返回中的公共字段

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
Ack| string类型|详情参看RESTful API [2.1 接口列表](urtc/cdnSteaming/cdnSteaming_RESTfulAPI?id=_21-接口列表)。
RetCode|int类型|错误代码，0 成功，非零代表失败，具体错误代码请参考错误代码总结。
Message|string类型|错误的文本提示。
Internal|json对象|不同Action需要携带的与频道、房间等配置有关的参数。
Data|json对象|根据不同的请求类型，data中的内容也不同，其中包含着具体请求结果的私有数据。

## 2. 旁路推流 调用时序

下图为实现 旁路推流 需要调用的 API 时序图。    

![](/images/cdnSteamingImage/cdnSteamingStart.png)

>查询、更新流、更新合流配置都是可选的，且可以多次调用，但是必须在旁路推流过程中（开始旁路推流后到结束旁路推流前）调用。

## 3 获取云端资源

在开始旁路推流之前，必须调用 job.acquire 方法请求一个用于旁路推流的 JobId 。每个 JobId 只能用于一次 旁路推流 的任务。

### 3.1 获取云端资源的请求

```json
{
    "Version": "1.0",
    "Action":"job.acquire",
    "Token": "xxxxxxx",
    "Internal": {
        "AppId": "xxx",
        "RoomId": "xxx",
        "Mode": 1,
        "ChannelType": 0/1,
    },
    "Data": { }
}
```

Internal       
    - AppId：string类型，开发者ID。AppId可从 URTC 产品中获取，可以参考[开通URTC服务](urtc/quick)。     
    - RoomId：string类型，录制的房间ID。    
    - ChannelType: int类型，0 会议模式（小班课）,1 直播模式（大班课）。    
- Data：空


### 3.2 获取云端资源的返回

```json
{
    "Version": "1.0",
    "Ack": "job.init",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "JobId": "xxx",
        "AppId": "xxx",
        "RoomId": "xxx",
        "Mode": 0/1,
        "ChannelType": 0/1,
    },
    "Data": {
        "JobId": "xxx"
    }
}
```

- Data
    - JobId：string类型，申请到的任务标识，**后续所有请求必须带上这个JobId**。


## 4. 开始旁路推流

### 4.1 开始旁路推流的请求

```json
{
    "Version": "1.0",
    "Action":"job.start",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxx",
        "AppId": "xxx",
        "RoomId": "xxx",
        "Mode": 0/1,
        "ChannelType": 0/1,
    },
    "Data": {
        "TranscodingConfig": {
            "Bitrate": 2000,
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 30,
                "Profile": "highprofile"
            },
            "Audio": {
                "Codec": "aac",
                "SampleRate": 48000
            }
        },
        "MixerConfig": {
            "MaxResolutionStream": “$userId_$mediaType”,
            "ResizeMode": 2,
            "MixedVideoLayout": 1,
        },
        "LiveConfig": {
            "Type": "rtmp",
            "Url": "rtmp://xxxxx"
        }
    }
}
```

### 4.2 开始旁路推流的返回

```json
{
    "Version": "1.0",
    "Ack":"job.stat",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "JobId": "xxx",
        "AppId": "xxx",
        "RoomId": "xxx",
        "Mode": 0/1,
        "ChannelType": 0/1,
    },
    "Data": {
        "TranscodingConfig": {
            "Bitrate": 2000,
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 30,
                "Profile": "highprofile"
            },
            "Audio": {
                "Codec": "aac",
                "SampleRate": 48000
            }
        },
        "MixerConfig": {
            "MaxResolutionStream": "user_type",
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
            "ResizeMode": 2,
            "MixedVideoLayout": 1,
        },
        "LiveConfig": {
            "Type": "rtmp",
            "Url": "rtmp://xxxxx"
        }
    }
}
```
字段具体标识请阅读 [配置参数详解](urtc/cdnSteaming/cdnSteaming_RESTfulAPI)。

# 8. 查询任务状态

## 8.1 请求
```json
{
    "Version": "1.0",
    "Action": "job.query",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxx",
    },
    "Data": {}
}
```
字段具体标识请阅读**配置参数详解**。

## 8.2 返回
```json
{
    "Version": "1.0",
    "Ack": "job.query.stat",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "JobId": "xxx",
        "AppId": "xxx",
        "RoomId": "xxx",
        "Mode": 0/1,
        "ChannelType": 0/1,
    },
    "Data": {
        "JobConfig": {
            "IdleTime": 60,
            "KeyStream": "user_type"
        },

        "TranscodingConfig": {
            "Bitrate": 1000,
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 15,
                "Profile": "baseline"
            },
            "Audio": {
                "Codec": "aac",
                "SampleRate": 48000
            }
        },

        "MixerConfig": {
            "MaxResolutionStream": "user_type",
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
            "Crop": false,
            "ResizeMode": 0/1/2,
            "MixedVideoLayout": 0/1/2/3/4,
            "WaterMark": {
                "Type": 1/2/3,
                "Image": "",
                "Text": "",
                "X": 10,
                "Y": 10,
                "Alpha": 0.1
            }
        },

        "SubscribeConfig": {
            "VideoStreamType": 0/1,
            "MaxSubscriptions": 32,
            "SubscribeAudio": ["", "", "", ""],
            "UnsubscribeAudio": ["", "", "", ""],
            "SubscribeVideo": ["", "", "", ""],
            "UnsubscribeVideo": ["", "", "", ""]
        },

        "RecordingConfig": {
            "MediaChannel": 2,
            "FileType": ["mp4", "webm"],
            "StorageConfig": {
                "PublicKey": "xxxxxxx",
                "SecretKey": "xxxxx",
                "Region": "xxx",
                "Bucket": "xxxxxx",
                "FileNamePrefix": ""
            }
        },

        "LiveConfig": {
            "Type": "rtmp",
            "Url": "rtmp://xxxxx"
        }
    }
}
```
字段具体标识请阅读**配置参数详解**。

# 6. 更新合流布局

## 6.1 请求

```json
{
    "Version": "1.0",
    "Action":"job.mixer.update",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxx",
    },
    "Data": {
        "MixerConfig": {
            "MaxResolutionStream": "user_type",
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
            "Crop": false,
            "ResizeMode": 0/1/2,
            "MixedVideoLayout": 0/1/2/3/4,
            "WaterMark": {
                "Type": 1/2/3,
                "Image": "",
                "Text": "",
                "X": 10,
                "Y": 10,
                "Alpha": 0.1
            },
        }
    }
}
```
字段具体标识请阅读**配置参数详解**。

## 6.2 返回

```json
{
    "Version": "1.0",
    "Ack": "job.mixer.stat",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "JobId": "xxx",
        "AppId": "xxx",
        "RoomId": "xxx",
        "Mode": 0/1,
        "ChannelType": 0/1,
    },
    "Data": {
        "JobConfig": {
            "IdleTime": 60,
            "KeyStream": "user_type"
        },

        "MixerConfig": {
            "MaxResolutionStream": "user_type",
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
            "Crop": false,
            "ResizeMode": 0/1/2,
            "MixedVideoLayout": 0/1/2/3/4,
            "WaterMark": {
                "Type": 1/2/3,
                "Image": "",
                "Text": "",
                "X": 10,
                "Y": 10,
                "Alpha": 0.1
            },
        }
    }
}
```
字段具体标识请阅读**配置参数详解**。

# 7. 更新流的状态

## 7.1 请求
```json
{
    "Version": "1.0",
    "Action": "job.stream.update",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxx",
    },
    "Data": {
    	"Stream": {
            "CmdType":1/2/3, 1 加流 2删流 3 mute
            "SubScribeId": "xxx_1"
            "HasVideo": true,
            "HasAudio": true,
            "MuteVideo": false,
            "MuteAudio": false
    	}
    }
}
```
字段具体标识请阅读**配置参数详解**。

## 7.2 响应
```json
{
    "Version": "1.0",
    "Action": "job.stream.status",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxx",
    },
    "Data": {
    	"Stream": {
            "CmdType":1/2/3, 1 加流 2删流 3 mute
            "SubScribeId": "xxx_1"
            "HasVideo": true,
            "HasAudio": true,
            "MuteVideo": false,
            "MuteAudio": false
    	}
    }
}
```
字段具体标识请阅读**配置参数详解**。



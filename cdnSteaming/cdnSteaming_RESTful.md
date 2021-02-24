# 旁路转推 RESTful API使用指南

# 1. 说明

- `restful api`使用POST接口类型，关于`鉴权token`的生成规则请参考  [Token生成指导](urtc/sdk/token)。

- `restful api`的http返回值永远是HTTP 200，所以不能根据HTTP 返回值判断指令是否成功，需要解析http body中的json内容判断指令是否成功。详细内容请参考下文。
- `restful api`中用`“$userId_$mediaType”`标记一路流：
	- userId：string类型， 是用户Id。
	- mediaType：int类型，是指摄像头流或桌面流，1 代表摄像头流，2 代表桌面流。

# 2. 旁路推转推 RESTFUL API

## 2.1 接口列表

接口 | 请求 | 返回 | 描述
--- | --- | --- | ---
获取资源 | job.acquire | job.init | 服务器分配任务资源，返回jobId，后续所有请求必须携带此处的`jobId`。<br>开多个任务时，需要调用多次，获取多个资源。
开启任务 | job.start | job.stat | 将任务开启，通过此命令可以开启`转推`功能。
开启录制 | job.record.start | job.record.stat | 在转推开启后再开启云端录制功能
关闭录制 | job.record.stop | job.record.stat | 在转推开启后再关闭云端录制功能
更新黑白名单 | job.subscribe.update | job.subscribe.stat | 更新订阅流的黑白名单，可以通过该命令传入白名单或者黑名单。<br>需要注意的是，白名单和黑名单不允许共存。
更新合流配置 | job.mixer.update | job.mixer.stat | 通过改命令更新合流模板，合流后的风格样式、水印等。
更新流 | job.stream.update | job.stream.stat | 通过改命令更新流，可以添加、删除流，mute以及非mute流状态。
开启消息通知 | job.notify.update | job.notify.stat | 更新消息通知服务状态
查询接口 | job.query | job.query.stat | 查询当前的任务参数信息
关闭任务 | job.stop | job.stat | 关闭`job`，回收资源。

> - 如果房间内只需要开启`转推`，则使用 `job.start` 开启转推；
> - 如果房间内已经了开启`转推`，则 `job.record.start` 开启录制；


## 2.2 请求中的公共字段

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
Action| 请求类型，详情参看上文 [2.1 接口列表](urtc/cdnSteaming/cdnSteaming_RESTful?id=_21-接口列表)。
Token| 鉴权Token，生成规则参考 [Token生成指导](urtc/sdk/token)。
Internal| 不同Action需要携带的与频道、房间等配置有关的参数。
Data| 不同Action需要携带的跟转码、合流、转推等配置有关的参数。


## 2.3 返回中的公共字段D

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
Ack| string类型|详情参看上文 [2.1 接口列表](urtc/cdnSteaming/cdnSteaming_RESTful?id=_21-接口列表)。
RetCode|int类型|错误代码，0 成功，非零代表失败，具体错误代码请参考错误代码总结。
Message|string类型|错误的文本提示。
Internal|json对象|不同Action需要携带的与频道、房间等配置有关的参数。
Data|json对象|根据不同的请求类型，data中的内容也不同，其中包含着具体请求结果的私有数据。

# 3. 获取云端资源

## 3.1 请求

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


## 3.2 返回

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


# 4. 开始旁路推流

## 4.1 请求

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
        "JobConfig": {
            "IdleTime": 60,
            "KeyStream": "user_type"
        },
        "NotifyConfig": [
        	{
                "Status": "open",
                "ServiceType": "job.living",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
        	},
        ],
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
            "ResizeMode": 0/1/2,
            "MixedVideoLayout":  0/1/2/3/4/5,
	
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

        "LiveConfig": {
            "Type": "rtmp",
            "Url": "rtmp://xxxxx"
        }
    }
}
```

## 4.2 返回

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
        "JobConfig": {
            "IdleTime": 60,
            "KeyStream": "user_type"
        },

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
            "ResizeMode": 0/1/2,
            "MixedVideoLayout":  0/1/2/3/4,
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

        "LiveConfig": {
            "Type": "rtmp",
            "Url": "rtmp://xxxxx"
        }
    }
}
```
字段具体标识请阅读**配置参数详解**。


<details>
	<summary>旁路推流开启后再开启录制</summary>

## 旁路推流开启后再开启录制

### 请求
```json
{
    "Version": "1.0",
    "Action":"job.record.start",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxx",
    },
    "Data": {
        "RecordingConfig": {
            "MediaChannel": 2,
            "FileType": ["mp4", "webm"],
            "StorageConfig": {
                "accessKey": "xxxxxxx",
                "Region": 1,
                "Bucket": "xxxxxx",
                "SecretKey": "xxxxx",
                "FileNamePrefix": ""
            }
        }
    }
}
```
字段具体标识请阅读**配置参数详解**。

### 返回

```json
{
    "Version": "1.0",
    "Ack":"job.record.stat",
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

        "RecordingConfig": {
            "MediaChannel": 2,
            "FileType": ["mp4", "webm"],
            "StorageConfig": {
                "Vendor": "ucloud",
                "PublicKey": "xxxxxxx",
                "SecretKey": "xxxxx",
                "Region": "xxx",
                "Bucket": "xxxxxx",
                "FileNamePrefix": ""
            }
        }
    }
}
```
字段具体标识请阅读**配置参数详解**。


## 停止云端录制

### 请求

```json
{
    "Version": "1.0",
    "Action":"job.record.stop",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxx",
    },
    "Data": {}
}
```
字段具体标识请阅读**配置参数详解**。

### 返回

```json
{
    "Version": "1.0",
    "Ack":"job.record.stat",
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

        "RecordingConfig": {
            "MediaChannel": 2,
            "FileType": ["mp4", "webm"],
            "StorageConfig": {
                "Vendor": "ucloud",
                "PublicKey": "xxxxxxx",
                "SecretKey": "xxxxx",
                "Region": "xxx",
                "Bucket": "xxxxxx",
                "FileNamePrefix": ""
            }
        }
    }
}
```
字段具体标识请阅读**配置参数详解**。


</details>

# 5. 更新订阅名单

## 5.1 请求

```json
{
    "Version": "1.0",
    "Action":"job.subscribe.update",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxx",
    },
    "Data": {
        "SubscribeConfig": {
            "VideoStreamType": 0/1,
            "MaxSubscriptions": 32,
            "SubscribeAudio": ["", "", "", ""],
            "UnsubscribeAudio": ["", "", "", ""],
            "SubscribeVideo": ["", "", "", ""],
            "UnsubscribeVideo": ["", "", "", ""]
        }
    }
}
```
字段具体标识请阅读**配置参数详解**。

## 5.2 返回

```json
{
    "Version": "1.0",
    "Ack":"job.subscribe.stat",
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

        "SubscribeConfig": {
            "VideoStreamType": 0/1,
            "MaxSubscriptions": 32,
            "SubscribeAudio": ["", "", "", ""],
            "UnsubscribeAudio": ["", "", "", ""],
            "SubscribeVideo": ["", "", "", ""],
            "UnsubscribeVideo": ["", "", "", ""]
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

# 9. 停止任务

## 9.1 请求
```json
{
    "Version": "1.0",
    "Action":"job.stop",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxx",
    },
    "Data": { }
}
```

## 9.2 返回

```json
{
    "Version": "1.0",
    "Ack": "job.distroied",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "JobId": "xxx",
        "AppId": "xxx",
        "RoomId": "xxx",
        "Mode": 0/1,
        "ChannelType": 0/1,
    },
    "Data": { }
}
```

# 10. 配置参数详解

## JobConfig: 任务的全局配置

- IdleTime：int类型，当房间内所有需要录制的流都断开后或者`KeyStream`指定的流断开后，`IdleTime`时间后服务器停止并回收任务。
- KeyStream：string类型，指定房间内的关键流，如果设置此项字段，则在此路流结束后任务会超时回收，超时时间以`IdleTime`为准。
`KeyStream`的生成规则是`$userid_$mediatype`，举个例子，用户`user001`的`摄像头流`可以用`user001_1`来表示，对应的`桌面流`可以用`user001_2`来表示。

## NotifyConfig：消息回调到用户服务器
   - NotifyUrl string类型，指定用户的消息服务地址
   - ServiceType string类型，指定模块的事件通知：旁路转推(job.living)。
   - EventType string类型，表示某个模块事件类型
   - Status string类型，表示要更新的状态  open 开启 close 关闭

## TranscodingConfig：转码配置

- Bitrate: int类型，转码后的码流。
- Video：视频编码信息。
  - Codec：string类型，编码类型，目前仅支持h264。
  - Width：int类型，分辨率，宽。
  - Height：int类型，分辨率，高。
  - Fps：int类型，帧率。
  - Profile：string类型，可选，`baseline`、`highprofile`。
- Audio：音频编码信息。
  - Codec：string类型，音频编码类型，目前仅支持aac。
  - SampleRate：int类型，音频采样率，目前仅支持 48khz。


## MixerConfig：合流配置

- MaxResolutionStream：string类型，指定合流模板中,最大分辨率的子画面的用户ID及媒体流的类型，`“$userId_$mediaType”`。
- BackgroundColor：json对象，背景色（RGB值），`{"R": 0, "G": 0, "B": 0}`代表黑色。
- ResizeMode：int类型，合流视频的显示策略 0 非等比拉伸 1裁剪 2 加黑边
- MixedVideoLayout：int类型，合流布局模板选择，可设置为：0-5。`0` 为自定义模板需参考`Layouts`中的模板信息。1-5分别代表：平铺、垂直、单画面、平铺2、垂直2。具体风格参照[混流风格](urtc/cloudRecord/RecordLaylout)。
- Layouts：array类型，这是一个二维数组，由不同画面数的模板组成的数组，只有当`MixedVideoLayout`为`0`时服务器才加载该参数，其他情况下可以不填此参数。
    - Id：合流区域标识，在同一个模板中不能重复。
    - Shape：合流区域的形状，目前仅支持矩形区域（`rectangle`）。
    - Area：子流显示区域的坐标和大小。
        - Left：string类型，用字符串表示的分数，屏幕里该画面左上角的横坐标的相对值，范围是 `[0/1, 1/1]`。从左到右布局，`0/1` 在最左端，`1/1` 在最右端。
        - Top：string类型 ，用字符串表示的分数，屏幕里该画面左上角的纵坐标的相对值，范围是 `[0/1, 1/1]`。从上到下布局，`0/1` 在最上端，`1/1` 在最下端。
        - Width：string类型，用字符串表示的分数，该画面宽度的相对值，取值范围是 `[0/1, 1/1]`。
        - Height：string类型，用字符串表示的分数，该画面高度的相对值，取值范围是 `[0/1, 1/1]`。
- Watermark：水印信息。
  - Type：int类型，0 不显示水印，1 时间水印，2 图片水印，3 文本水印
  - Image：string类型，水印图片存放的http地址，不可与`Text`同用。
  - Text：string类型，水印文字的内容，不可与`Image`同用。
  - X：int类型，屏幕里该画面左上角的横坐标的坐标值，范围是 `[0 - MaxWidth]`。Maxwidth是指TranscodingConfig中的width.
  - Y：int类型，屏幕里该画面左上角的纵坐标的坐标值，范围是 `[0 - MaxHeight]`。MaxHeight是指TranscodingConfig中的Height。
  - Alpha：int类型，透明度，浮点型，取值范围 0 ~ 1。


## SubscribeConfig：订阅流配置

- MaxSubscriptions: int类型，一个房间里最大的录像流数目，默认 32。
- SubscribeAudio: array类型，音频白名单，除了该名单以外的流都不录制音频，不能与`UnsubscribeAudio`共用。
- SubscribeVideo: array类型，视频白名单，除了该名单以外的流都不录制视频，不能与`UnsubscribeVideo`共用。
- UnsubscribeAudio: array类型，音频黑名单，除了该名单以外的流都要录制音频，不能与`SubscribeAudio`共用。
- UnsubscribeVideo: array类型，视频黑名单，除了该名单以外的流都要录制视频，不能与`SubscribeVideo`共用。

## LiveConfig: 转推配置

- Type： string类型， 转推的协议类型
- Url: string类型， 转推服务器的地址


# 11. 更新消息通知服务

## 描述

RESTful API 提供消息通知服务，用户可以指定模块消息通知到用户的消息服务器地址，用户请求的JSON 方法如下所示。

## 11.1 开启消息通知

### 请求

 - NotifyConfig
   - NotifyUrl string类型，指定用户的消息服务地址
   - ServiceType string类型，指定模块的事件通知：旁路转推(job.living)。
   - EventType string类型，表示某个模块事件类型
   - Status string类型，表示要更新的状态  open 开启 close 关闭

 ```json
 {
     "Version": "1.0",
     "Action":"job.notify.update",
     "Token": "xxxxxxx",
     "Internal": {
         "JobId": "xxx",
     },
     "Data": {
    	 "NotifyConfig": [
        	{
                "Status": "open",
                "ServiceType": "job.living",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
        	},
        ]
 }
 ```
 
 ### 11.2 返回
 
 ```json
 {
     "Version": "1.0",
     "Ack":"job.notify.stat",
     "RetCode": 0,
     "Message": "OK",
     "Internal": {
         "JobId": "xxx"
     },
     "Data": {
    	 "NotifyConfig": [
        	{
                "Status": "open",
                "ServiceType": "job.living",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
        	},
        ]
 }
 ```


# 12. 回调通知

## 描述

RESTful API 提供消息通知服务，用户可以配置一个接收回调的HTTP/HTTPS 服务器地址来接收事件通知。    
  
当有事件需要回调通知时，可以通过HTTP/HTTPS 请求的方式将事件投递给用户的消息服务器。

### 12.1 通知请求

   ```json
  {
      "Version": "1.0",
      "Token": "xxxxxxx",
      "ServiceType": "job.living",
      "EventType": "ucloud_record_error",
      "Internal": {
          "JobId": "xxx"
      },
      "Data": {
        "ErrorCode": 0,
        "ErrorMsg": "record started successfully"
     }
  ```
Data 字段中的ServiceType, EventType 为请求包体重公共字段，所有回调重都包含这些字段，公共字段的含义详见 消息回调事件。
   
## 12.2 消息回调事件

EvenType | ServiceType 		| 事件描述
------ |   -----------   	| ---
ucloud_living_status_update 	| job.living  (转推服务)		| 转推服务状态发生变化
ucloud_living_error	   			| job.living  (转推服务)		| 转推服务发生错误
ucloud_living_warning     		| job.living  (转推服务)		| 转推服务发生警告
ucloud_living_stream_update  	| job.living  (转推服务）	    | 转推服务流发生变化
ucloud_living_session_failover 	| job.living  (转推服务）	    | 转推服务启用高可用


## 12.3 错误码说明

  - ucloud_session_failover.
  	EventType 表示服务启动高可用。启动高可用之后，服务端崩溃之后会重新开启一个以原始JodId为标识的服务，默认开启。
    - Status: int 类型， 0 启用成功 1 启用失败。
	
  - ucloud_living_status_update
      EventType 表示转推服务状态发生变化
    - Status: int 类型， 云端录制当前状态，请参考[服务状态码](urtc/cdnSteaming/cdnSteaming_RESTful?id=_124-服务状态码)。

  - ucloud_living_warning.
      EventType 表示转推服务状态发生变化
      - WarnCode: int 类型，警告码，根据当前服务模块类型，查看具体模块的[告警码](urtc/cdnSteaming/cdnSteaming_RESTful?id=_126-告警码)。

  - ucloud_living_error.
  	 EventType 表示转推服务发生错误
    - ErrorCode: int 类型，错误码。根据当前服务模块类型，查看具体模块的[错误码](urtc/cdnSteaming/cdnSteaming_RESTful?id=_125-错误码)。
    - ErrorMsg: string 类型，具体的事件信息。

  - ucloud_living_stream_update.
  	 EventType 表示转推服务视频流状态发生变化。
    - StreamUid: string类型，表示当前更新流的uid.
    - Status: string 类型，open 表示开始接收 close 表示停止接收
    - TimeStamp: string 类型，当前操作的时间戳。

  - ucloud_mixer_status_update.
      EventType 表示合流录制服务状态发生变化
    - Status: int 类型， 云端录制当前状态，请参考模块状态服务码。

  - ucloud_mixer_warning.
      EventType 表示合流录制服务状态发生变化
      - WarnCode: int 类型，警告码，根据当前服务模块类型，查看具体模块的[告警码](urtc/cdnSteaming/cdnSteaming_RESTful?id=_125-错误码)。

  - ucloud_mixer_error.
  	 EventType 表示合流录制服务发生错误
    - ErrorCode: int 类型，错误码。根据当前服务模块类型，查看具体模块的[错误码](urtc/cdnSteaming/cdnSteaming_RESTful?id=_125-错误码)。
    - ErrorMsg: string 类型，具体的事件信息。

  - ucloud_mixer_stream_update.
  	 EventType 表示合流录制服务视频流状态发生变化。
    - StreamUid: string类型，表示当前更新流的uid.
    - Status: string 类型，open 表示开始接收 close 表示停止接收
    - TimeStamp: string 类型，当前操作的时间戳。

## 12.4 服务状态码

状态码  		| 描述
------ |  ---
SERVICE_STATUS_IDEL      		| 服务没有开启
SERVICE_STATUS_RUNNING      	| 服务正在运行
SERVICE_STATUS_EXITING      	| 服务正常退出
SERVICE_STATUS_INTERRUPT    	| 服务中断退出

## 12.5 错误码

错误码 | 枚举值 		| 描述
------ |   -----------   	| ---
ERROR_OK      						| 0		| 没有错误
ERROR_FAILED       					| 1		| 一般性错误。没有明确归类的错误原因
ERROR_JOB-EXISTENT      		·	| 2		| 任务已开启
ERROR_JOB_NON-EXISTENT      		| 3		| 任务未开启
ERROR_MIXERED_INVALID_VIDEO_PARAM   | 4		| 服务收到无效的视频合流参数
ERROR_MIXERED_INVALID_AUDIO_PARAM  	| 5		| 服务收到无效的音频合流参数
ERROR_MIXERED_STREAM_NON_EXISTENT	| 6		| 服务指定流信息不存在

## 12.6 告警码

告警码 |  描述
------ |  ---
SERVICE_WARN_PROCESS_RESTART    	| 任务异常重启


 

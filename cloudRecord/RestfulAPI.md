# 云端录制 Restful API接口

## 1. 服务说明

### 1.1 云录制

云录制有两个模式：`合流录制` 和 `单流录制`。

- 合流录制：将房间里的多条流合并成一路视频文件，并且允许控制接口设置合流模板。
- 单流录制：将房间里的多条流独立录制成文件。

### 1.2 订阅流的标记

restful api 中用`“$userId_$mediaType”`标记一路流：
- userId：string类型， 是用户Id。
- mediaType：int类型，是指摄像头流或桌面流，1 代表摄像头流，2 代表桌面流。

```
例如：用户user001 的摄像头流用 user001_1，桌面流用 user001_2 表示。
```

### 1.3 注意项

- `job`开启之后，`单流录制`不能和`合流录制`功能共同使用。
但是可以同时启动两个`job`，一个`job`用来`单流录制`，一个`job`用来`合流录制`。

- `restful api`使用POST接口类型，关于`鉴权token`的生成规则请参考 [https://docs.ucloud.cn/urtc/sdk/Token](https://docs.ucloud.cn/urtc/sdk/Token)

- `restful api`的http返回值永远是HTTP 200，所以不能根据HTTP 返回值判断指令是否成功，需要解析http body中的json内容判断指令是否成功。详细内容请参考下文。

---

## 2. 录制Restful API

### 2.1 接口列表

接口 | 请求 | 返回 | 描述
--- | --- | --- | ---
获取资源 | job.acquire | job.init | 服务器分配任务资源，返回jobId，后续所有请求必须携带此处的jobId。
开启任务 | job.start | job.stat | 将任务开启，通过此命令可以指定是否使用`合流录制`、`转推`、`单流录制`功能。当然，你也可以在此阶段只开启其中一项功能，后面通过下面的接口开启你需要的功能。
开启云存 | job.record.start | job.record.stat | 开启云端录制功能
关闭云存 | job.record.stop | job.record.stat | 关闭云端录制功能
更新订阅流名单 | job.subscribe.update | job.subscribe.stat | 更新订阅流名单，可以通过该命令传入白名单或者黑名单，需要注意的是，白名单和黑名单不允许共存。
更新转码参数 | job.transcoding.update | job.transcoding.stat | 更新转码参数，通过次命令可以更流合流的码率 宽 高 帧率等信息。
更新合流配置 | job.mixer.update | job.mixer.stat | 通过改命令更新合流模板，以及合流后的样式、水印等。
批量更新操作 | job.update | job.update.stat | 通过改命令更新任务参数，支持对应ActionId的有 `job.subscribe.update`，`job.transcoding.update`，`job.mixer.update`，`job.stream.update`，`job.notify.update`等接口.
开启消息通知 | job.notify.update | job.notify.stat | 更新消息通知服务状态
查询接口 | job.query | job.query.stat | 查询当前的任务参数信息
关闭任务 | job.stop | job.destroyed | 关闭`job`，回收资源。


---


### 2.2 请求中的公共字段

```json
{
    "Version": "1.0",
    "Action":"xxx",
    "": "",
    "Internal": {
        "RequestId": "0"
    },
    "Data": {
        ...
    }
}
```

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
Version     | string类型    | 选填  | 服务版本，如果后台服务版本升级，可通过此字段完成向前兼容，当前服务版本`1.0`。
Action      | string类型    | 必填  | 请求类型，详情参看上文 [2.1 接口列表](#)。
Internal    | json对象      | 必填  | 不同Action需要携带的与频道、房间等配置有关的参数。
Data        | json对象      | 必填  | 不同Action需要携带的跟录制、转码、合流、直播等配置有关的参数。


---

### 2.3 返回中的公共字段D

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
参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
Version     | string类型    | 选填  | 服务版本，如果后台服务版本升级，可通过此字段完成向前兼容，当前服务版本`1.0`。
Ack         | string类型    | 必填  | 请求类型，详情参看上文 [2.1 接口列表](#)。
RetCode     | int类型       | 必填  | 错误代码，0 成功，非零代表失败，具体错误代码请参考错误代码总结。
Message     | string类型    | 必填  | 错误的文本提示。
Internal    | json对象      | 选填  | 不同Action需要携带的与频道、房间等配置有关的参数。
Data        | json对象      | 必填  | 根据不同的请求类型，data中的内容也不同，其中包含着具体请求结果的私有数据。

---


## 3. 获取云端资源

### 3.1 请求

```json
{
    "Version": "1.0",
    "Action":"job.acquire",
    "Token": "xxxxxxx",
    "Internal": {
        "RequestId": "0",
        "AppId": "xxx",
        "RoomId": "xxx",
        "Mode": 0/1,
        "ChannelType": 0/1,
    },
    "Data": { }
}
```

- Internal 参数描述

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
AppId       | string类型    | 必填  | 开发者ID。AppId可从 URTC 产品中获取，可以参考开通[URTC服务](https://docs.ucloud.cn/urtc/quick)。
RoomId      | string类型    | 必填  | 房间的ID。
Mode        | int类型       | 必填  | 任务模式，0 单流录制模式，1 合流模式。
ChannelType | int类型       | 必填  | 0 会议模式（小班课）,1 直播模式（大班课）。
Data        | json对象      | 选填  | 根据不同的请求类型，data中的内容也不同，其中包含着具体请求结果的私有数据。
RequestId   | string类型    | 必填   | 请求的标识ID



### 3.2 返回

```json
{
    "Version": "1.0",
    "Ack": "job.init",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "RequestId": "0",
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
- body 内容描述

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
AppId       | string类型    | 必填  | 开发者ID。AppId可从 URTC 产品中获取，可以参考开通[URTC服务](https://docs.ucloud.cn/urtc/quick)。
RoomId      | string类型    | 必填  | 房间的ID。
Mode        | int类型       | 必填  | 任务模式，0 单流录制模式，1 合流模式。
RequestId   | string类型    | 必填   | 请求的标识ID
ChannelType | int类型       | 必填  | 0 会议模式（小班课）,1 直播模式（大班课）。
Data        | json对象      | 必填  | 根据不同的请求类型，data中的内容也不同，其中包含着具体请求结果的私有数据。

- Data中字段描述

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
JobId       | string类型    | 必填  | 申请到的任务标识，后续所有请求必须带上这个。

---



## 4. 开始云端录制

### 4.1 开始云端录制的请求

```json
{
    "Version": "1.0",
    "Action":"job.start",
    "Token": "xxxxxxx",
    "Internal": {
        "RequestId": "0",
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
                "ServiceType": "job.record",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
        	},
            {
                "Status": "open",
                "ServiceType": "job.oss",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
        	}
        ],
        "TranscodingConfig": {
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 15,
                "BitRate": 1000,
                "Profile": "baseline"
            },
            "Audio": {
                "Codec": "aac",
                "SampleRate": 4800
            }
        },
        "MixerConfig": {
            "MaxResolutionStream": "user_type",
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
            "ResizeMode": 0/1/2,
            "MixedVideoLayout":  0/1/2/3/4/5,
            "PositionConfig":[
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 0,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                },
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 960,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                }
            ],
            "LayoutConfig": [
                [
                    {
                        "Id": "1_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1",
                            "Height": "1"
                        }
                    }
                ],
                [
                    {
                        "Id": "2_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    },
                    {
                        "Id": "2_2",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "1/2",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    }
                ]
            ],
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
                "Vendor": "ucloud",
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
字段具体标识请阅读配置参数详解。


### 4.2 开始云端录制的返回

```json
{
    "Version": "1.0",
    "Ack":"job.stat",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "RequestId": "0",
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
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 15,
                "BitRate": 1000,
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
            "ResizeMode": 0/1/2,
            "MixedVideoLayout":  0/1/2/3/4,
            "PositionConfig":[
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 0,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                },
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 960,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                }
            ],
            "LayoutConfig": [
                [
                    {
                        "Id": "1_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1",
                            "Height": "1"
                        }
                    }
                ],
                [
                    {
                        "Id": "2_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    },
                    {
                        "Id": "2_2",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "1/2",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    }
                ]
            ],
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
                "Vendor": "ucloud",
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
字段具体标识请阅读配置参数详解。

---



## 5. 更新云端录制的用户信息

### 5.1 更新云端录制的用户的请求

```json
{
    "Version": "1.0",
    "Action":"job.subscribe.update",
    "Token": "xxxxxxx",
    "Internal": {
        "RequestId": "0",
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
字段具体标识请阅读配置参数详解。

### 5.2 更新云端录制的用户的返回

```json
{
    "Version": "1.0",
    "Ack":"job.subscribe.stat",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "RequestId": "0",
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
字段具体标识请阅读配置参数详解。



## 6. 更新合流录制的布局

### 6.1 更新合流录制的请求

```json
{
    "Version": "1.0",
    "Action":"job.mixer.update",
    "Token": "xxxxxxx",
    "Internal": {
        "RequestId": "0",
        "JobId": "xxx",
    },
    "Data": {
        "MixerConfig": {
            "MaxResolutionStream": "user_type",
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
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
            "PositionConfig":[
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 0,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                },
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 960,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                }
            ],
            "LayoutConfig": [
                [
                    {
                        "Id": "1_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1",
                            "Height": "1"
                        }
                    }
                ],
                [
                    {
                        "Id": "2_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    },
                    {
                        "Id": "2_2",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "1/2",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    }
                ]
            ]
        }
    }
}
```
具体参数说明请查询配置参数详解。

### 6.2 更新合流录制的返回

```json
{
    "Version": "1.0",
    "Ack": "job.mixer.stat",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "RequestId": "0",
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
            "PositionConfig":[
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 0,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                },
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 960,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                }
            ],
            "LayoutConfig": [
                [
                    {
                        "Id": "1_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1",
                            "Height": "1"
                        }
                    }
                ],
                [
                    {
                        "Id": "2_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    },
                    {
                        "Id": "2_2",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "1/2",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    }
                ]
            ]
        }
    }
}
```
字段具体标识请阅读配置参数详解。

## 7 更新合流录制的视频参数
### 7.1 更新合流录制的视频参数的请求
目前支持动态修改视频的分辨率 帧率 码率等。
```json
{
    "Version": "1.0",
    "Action":"job.transcoding.update",
    "Token": "xxxxxxx",
    "Internal": {
        "RequestId": "0",
        "JobId": "xxx",
    },
    "Data": {
        "TranscodingConfig": {
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 15,
                "BitRate": 1000,
                "Profile": "baseline"
            },
            "Audio": {
                "Codec": "aac",
                "SampleRate": 48000
            }
        }
    }
}
```

字段具体标识请阅读配置参数详解。

### 7.2 更新合流录制的视频参数的返回

```json
{
    "Version": "1.0",
    "Action":"job.transcoding.stat",
    "Token": "xxxxxxx",
    "Internal": {
        "RequestId": "0",
        "JobId": "xxx",
    },
    "Data": {
        "TranscodingConfig": {
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 15,
                "BitRate": 1000,
                "Profile": "baseline"
            },
            "Audio": {
                "Codec": "aac",
                "SampleRate": 48000
            }
        }
    }
}
```

字段具体标识请阅读配置参数详解。


## 8. 批量更新录制

### 8.1 批量更新录制的请求

```json
{
    "Version": "1.0",
    "Action":"job.update",
    "Token": "xxx",
    "Internal": {
        "JobId": "URtc-h4r1txxy-testa-1-1620888861-89"
    },
    "Data": {
        "NotifyConfig": [ 
            {
                "Status": "open",
                "ServiceType": "job.record",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
            },
            {
                "Status": "open",
                "ServiceType": "job.oss",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
            },
        ],
        "StreamConfig": 
        [
            {
                "RtcStreamId":"m9j73v4a_1",
                "HasVideo": true,
                "HasAudio": true,
                "MuteVideo": false,
                "MuteAudio": false
            },
            {
                "RtcStreamId":"sxqnb12w_1",
                "HasVideo": true,
                "HasAudio": true,
                "MuteVideo": false,
                "MuteAudio": false
            }
        ],
        "TranscodingConfig": {
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 15,
                "BitRate": 1000,
                "Profile": "baseline"
            },
            "Audio": {
                "Codec": "aac",
                "SampleRate": 48000
            }
        },
        "SubscribeConfig": {
            "VideoStreamType": 1,
            "MaxSubscriptions": 16,
            "SubscribeAudio": [],
            "UnsubscribeAudio": [],
            "SubscribeVideo": [],
            "UnsubscribeVideo": []
        },
        "MixerConfig": {
            "MaxResolutionStream": "shane_1",
            "BackgroundColor": {"R": 255, "G": 255, "B": 0},
            "ResizeMode": 2,
            "MixedVideoLayout": 2,
            "PositionConfig":[
                {
                    "RtcStreamId":"m9j73v4a_1",
                    "Region": {
                        "X":160,
                        "Y":90,
                        "Z":0,
                        "Width":240,
                        "Height":160
                    }
                },
                {
                    "RtcStreamId": "sxqnb12w_1",
                    "Region": {
                        "X":0,
                        "Y":0,
                        "Z":0,
                        "Width":160,
                        "Height":90
                    }
                }
            ],
            "WaterMark": {
                "Type": 1,
                "Image": "http://urtcdemo.ufile.ucloud.com.cn/urtc_icon.png",
                "Text": "this is a text WaterMark.",
                "X": 0,
                "Y": 0,
                "Alpha": 0.5
            },
            "LayoutConfig": []
        }
    }
}
```

job.update 更新接口，如果客户需要更新哪个子选项，可以在Data字段中带上所要更新的子项，如果不填默认不更新。    
目前支持支持更新的接口有 `job.subscribe.update`，`job.transcoding.update`，`job.mixer.update`，`job.notify.update`等接口.

### 8.2 批量更新录制的返回

```json
{
    "Version": "1.0",
    "Ack": "job.update.stat",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "RequestId": "0",
        "JobId": "xxx"
    },
    "Data": {
        "NotifyConfig": [ 
            {
                "Status": "open",
                "ServiceType": "job.record",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
            },
            {
                "Status": "open",
                "ServiceType": "job.oss",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
            },
        ],
        "StreamConfig": 
        [
            {
                "RtcStreamId":"m9j73v4a_1",
                "HasVideo": true,
                "HasAudio": true,
                "MuteVideo": false,
                "MuteAudio": false
            },
            {
                "RtcStreamId":"sxqnb12w_1",
                "HasVideo": true,
                "HasAudio": true,
                "MuteVideo": false,
                "MuteAudio": false
            }
        ],
        "TranscodingConfig": {
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 15,
                "BitRate": 1000,
                "Profile": "baseline"
            },
            "Audio": {
                "Codec": "aac",
                "SampleRate": 48000
            }
        },
        "SubscribeConfig": {
            "VideoStreamType": 1,
            "MaxSubscriptions": 16,
            "SubscribeAudio": [],
            "UnsubscribeAudio": [],
            "SubscribeVideo": [],
            "UnsubscribeVideo": []
        },
        "MixerConfig": {
            "MaxResolutionStream": "shane_1",
            "BackgroundColor": {"R": 255, "G": 255, "B": 0},
            "ResizeMode": 2,
            "MixedVideoLayout": 2,
            "PositionConfig":[
                {
                    "RtcStreamId":"m9j73v4a_1",
                    "Region": {
                        "X":160,
                        "Y":90,
                        "Z":0,
                        "Width":240,
                        "Height":160
                    }
                },
                {
                    "RtcStreamId": "sxqnb12w_1",
                    "Region": {
                        "X":0,
                        "Y":0,
                        "Z":0,
                        "Width":160,
                        "Height":90
                    }
                }
            ],
            "WaterMark": {
                "Type": 1,
                "Image": "http://urtcdemo.ufile.ucloud.com.cn/urtc_icon.png",
                "Text": "this is a text WaterMark.",
                "X": 0,
                "Y": 0,
                "Alpha": 0.5
            },
            "LayoutConfig": []
        }
    }
}
```


## 9. 查询录制任务状态

### 9.1 查询录制任务状态的请求
```json
{
    "Version": "1.0",
    "Action": "job.query",
    "Token": "xxxxxxx",
    "Internal": {
        "RequestId": "0",
        "JobId": "xxx",
    },
    "Data": {}
}
```
字段具体标识请阅读配置参数详解。

### 9.2 查询录制任务状态的返回
```json
{
    "Version": "1.0",
    "Ack": "job.query.stat",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "RequestId": "0",
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
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 15,
                "BitRate": 1000,
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
            "ResizeMode": 0/1/2,
            "MixedVideoLayout": 0/1/2/3/4,
            "PositionConfig":[
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 0,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                },
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 960,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                }
            ],
            "LayoutConfig": [
                [
                    {
                        "Id": "1_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1",
                            "Height": "1"
                        }
                    }
                ],
                [
                    {
                        "Id": "2_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    },
                    {
                        "Id": "2_2",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "1/2",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/2"
                        }
                    }
                ]
            ],
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
字段具体标识请阅读配置参数详解。


## 10. 停止录制任务

### 10.1 停止录制任务的请求
```json
{
    "Version": "1.0",
    "Action":"job.stop",
    "Token": "xxxxxxx",
    "Internal": {
        "RequestId": "0",
        "JobId": "xxx",
    },
    "Data": { }
}
```

### 10.2 停止录制任务的返回

```json
{
    "Version": "1.0",
    "Ack": "job.destroyed",
    "RetCode": 0,
    "Message": "OK",
    "Internal": {
        "RequestId": "0",
        "JobId": "xxx",
        "AppId": "xxx",
        "RoomId": "xxx",
        "Mode": 0/1,
        "ChannelType": 0/1,
    },
    "Data": { }
}
```

## 11. 配置参数详解

### JobConfig: 任务的全局配置

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
IdleTime    | int类型       | 选填  |当房间内所有需要录制的流都断开后或者`KeyStream`指定的流断开后，`IdleTime`时间后服务器停止并回收任务，默认60s。
KeyStream   |string类型     |选填   |指定房间内的关键流，如果设置此项字段，则在此路流结束后任务会超时回收，超时时间以`IdleTime`为准。


### SubscribeConfig：订阅流配置

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
VideoStreamType     | int类型       | 必填  | 合流任务的加流模式， 0 `自动模式`，1 `手动模式`。
MaxSubscriptions    | int类型       | 选填  | 一个房间里最大的录像流数目，默认 32。
SubscribeAudio      | arrary类型    | 选填  | 音频白名单，除了该名单以外的流都不录制音频，不能与`UnsubscribeAudio`共用。
SubscribeVideo      | array类型     |选填   | 视频白名单，除了该名单以外的流都不录制视频，不能与`UnsubscribeVideo`共用。
UnsubscribeAudio    |array类型      |选填   | 音频黑名单，除了该名单以外的流都要录制音频，不能与`SubscribeAudio`共用。
UnsubscribeVideo    |array类型      |选填   | 视频黑名单，除了该名单以外的流都要录制视频，不能与`SubscribeVideo`共用。

### RecordingConfig：云录制配置

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
MediaChannel        |int类型    |必填   | 1 只录音频，2 只录视频，3 音视频都录。
FileType            |array类型  |必填   | 允许服务器生成的文件类型，如果同时设置了`mp4`、`webm`，单流模式下如果视频编码为h264则生成mp4文件，如果视频编码为vp8则生成webm文件。`(目前mode=0 单流录制模式下必须设置为 mp4和webm并存)`.
Vendor              |string类型 |选填   | 云录制厂商，目前仅支持ucloud。
PublicKey           |string类型 |必填   | 云录制公钥，请参照us3文档真实填写。
SecretKey           |string类型 |必填   | 云录制私钥，请参照ufile文档真实填写。
Region              |string类型 |必填   | 云录制的region，请参照ufile文档真实填写。
Bucket              |string类型 |必填   | 云录制的bucket，请参照ufile文档真实填写。
FileNamePrefix      |string类型 |必填   | 单流录制指文件在ufile中的存储位置。举个例子，fileNamePrefix/stream.webm，合流录制指是生成的文件名。可以是数字，字母，下划线和特殊字符。

### TranscodingConfig：转码配置

- Video：视频编码信息。

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
Codec   |string类型 |必填   |编码类型，目前仅支持h264。
Width   |int类型    |必填   |分辨率，宽。
Height  |int类型    |必填   |分辨率，高。
Fps     |int类型    |必填   |帧率。
BitRate |int类型    |必填   |转码后的码流。
Profile |string类型 |必填   |可选，`baseline`、`highprofile`, 目前只支持`highprofile`。

- Audio：音频编码信息。

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
Codec        |string类型 |必填   |音频编码类型，合流任务转推rtmp协议或者录制支持aac，转推到RTC房间支持opus。
SampleRate   |int类型    |必填   |音频采样率，目前仅支持 48khz。

### MixerConfig：合流配置

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
MaxResolutionStream        |string类型 |选填   |指定合流模板中最大分辨率的子画面显示的流，如`user1_type`。
BackgroundColor            |json对象   |选填   |默认 `{"R": 0, "G": 0, "B": 0}`代表黑色, 只支持开启任务时候指定，中间更新不支持。
ResizeMode                 |int类型    |选填   |合流视频的显示策略 0 非等比拉伸 1裁剪 2 加黑边， 默认2。
MixedVideoLayout           |int类型    |必填   |合流布局模板选择，`0` 为自定义模板需参考，其他风格模板请查阅,[https://docs.ucloud.cn/urtc/cloudRecord/RecordLaylout]。
Layouts                    |array类型  |选填   |这是一个二维数组，由不同画面数的模板组成的数组，只有当`MixedVideoLayout`为`0`时服务器才加载该参数，其他情况下可以不填此参数。
PositionConfig             |json对象  |选填   |合成画面每条流的显示位置信息，仅对手动模式有效，自动模式下可以不填。
WaterMark                  |array类型  |选填   |水印信息，默认不加载。

- Layouts 参数描述

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
Id        |string类型 |必填   |合流区域标识，在同一个模板中不能重复。
Shape     |string类型 |必填   |合流区域的形状，目前仅支持矩形区域（`rectangle`）。
Area      |array类型  |必填   |合流区域标识，包括坐标，窗口大小等。

- Layouts中Area 参数描述

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
Left        |string类型 |必填   |用字符串表示的分数，屏幕里该画面左上角的横坐标的相对值，范围是 `[0/1, 1/1]`。从左到右布局，`0/1` 在最左端，`1/1` 在最右端。
Top         |string类型 |必填   |，用字符串表示的分数，屏幕里该画面左上角的纵坐标的相对值，范围是 `[0/1, 1/1]`。从上到下布局，`0/1` 在最上端，`1/1` 在最下端。
Width       |string类型 |必填   |用字符串表示的分数，该画面宽度的相对值，取值范围是 `[0/1, 1/1]`。
Height      |string类型 |必填   |用字符串表示的分数，该画面宽度的相对值，取值范围是 `[0/1, 1/1]`。


- PositionConfig 参数描述

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
RtcStreamId        |string类型 |必填   |当前某条流标识，用`“$userId_$mediaType”`。
Region     |json对象 |必填   | 描述某条流的显示位置信息

- PositionConfig 中Region 参数描述

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
X        |int类型 |必填   |屏幕里该画面左上角的横坐标值，仅支持4对齐，范围是 `[0 width]`。
Y        |int类型 |必填   |屏幕里该画面左上角的纵坐标值，仅支持4对齐，范围是 `[0 height]`。
Z        |int类型 |必填   |屏幕里该画面层叠关系，范围是 `[0 MaxSubscriptions]`, 暂时不支持。
Width    |int类型 |必填   |屏幕里该画面窗口宽度，仅支持4对齐，范围是 `[0 width]`。
Height   |int类型 |必填   |屏幕里该画面窗口高度，仅支持4对齐，范围是 `[0 height]`。

- WaterMark参数描述。

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
Type      |int类型      |必填   |0 不显示水印，1 时间水印，2 图片水印，3 文本水印，支持`[0,3]`.
Image     |string类型   |必填   |水印图片存放的http地址，不可与`Text`同用。
Text      |string类型   |必填   |水印文字的内容，不可与`Image`同用。
X         |int类型      |必填   |屏幕里该画面左上角的横坐标的坐标值，范围是 `[0 - MaxWidth]`。Maxwidth是指TranscodingConfig中的width.
Y         |int类型      |必填   |屏幕里该画面左上角的纵坐标的坐标值，范围是 `[0 - MaxHeight]`。MaxHeight是指TranscodingConfig中的Height。
Alpha     |int类型      |必填   |透明度，浮点型，取值范围 0 ~ 1,目前不支持。

- StreamConfig：流信息描述。

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
RtcStreamId   |string类型 |必填   |标识某一条的流的ID，用`“$userId_$mediaType”`。
HasVideo      |bool类型    |必填   |是否有视频。
MuteVideo     |bool类型    |必填   |视频的状态。
HasAudio      |bool类型    |必填   |是否有视频。
MuteAudio     |bool类型    |必填   |音频的状态。


## 11. 单流录制规则

### 11.1 单流录制的功能描述

- 单流录制模式会将属于同一个房间的所有流都录制下来，h264编码的流存储格式为mp4，vp8编码的流存储格式为webm。
- 为了描述每个录像文件的先后时间关系，每个单流录制任务都会生成一个索引文件，用来记录这次录制任务下每条流的起始时间和结束时间。离线合流工具通过这个索引文件就可将所有这次房间里的流按照时间顺序合为一个完整的视频文件。

### 11.2 索引格式

```json
{
    "JobId": "xxx",
    "RoomId": "xxx",
    "StartTime": xxx,
    "Root": "dir1/dir2",
    "Files": [
        {
            "StreamId": "$user_$Type",
            "Name": "xxx.mp4",
            "StartTime": xxx
        },
        {
            "StreamId": "$user_$Type",
            "Name": "xxx.webm",
            "StartTime": xxx
        },
        ...
    ]
}
```

## 12. 离线合流

### 12.1 离线合流的功能描述

离线合流需要从ufile下载录像文件，将多个uid对应的单路录制文件以画中画的方式合成一个录制文件，可以使用我们音视频合并转码的工具，具体需要以下几个步骤：

- 从ufile上获取每个uid对应的录制文件和房间记录所有流信息的info文件。
- 将多个单流文件进行转码合成一个完成画中画录制文件。
- 将生成的画中画录制文件上传到ufile上。

### 12.2 用户使用工具方法
 登录部署好的机器，默认部署在/home/urtc-owt/目录下，具体下面包括以下几个子目录。
 - bin :  可执行工具目录（不可删除，不可修改）
 - conf:  任务启动需要参数的配置目录(不可删除，不可修改)
 - logs： 日志目录
 - mixer: 用户传入配置文件目录(用户可修改)

用户执行以下命令后，合并任务开启运行，任务结束自动退出：
- ./bin/owt-main ./mixer/cfg.json  &
   
 
在用户使用转码合并工具之前，用户需要修改用户的合流信息文件，具体参数依据用户实际使用场景。
```json
{
    "Version": "1.0",
    "Action":"job.start",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "URtc-h4r1txxy-123-yInVQgRk",
        "AppId": "URtc-h4r1txxy",
        "RoomId": "123",
        "Mode": 1,
        "ChannelType": 1
    },
    "Data": {
        "JobConfig": {
            "IdleTime": 60,
            "KeyStream": "xxx_1"
        },

        "TranscodingConfig": {
            "Video": {
                "Codec": "h264",
                "Width":1920,
                "Height": 1080,
                "Fps": 15,
                "BitRate": 1000,
                "Profile": "baseline"
            },
            "Audio": {
                "Codec": "aac_48000_2",
                "SampleRate": 48000
            }
        },

        "MixerConfig": {
            "MaxResolutionStream": "PR5012_1",
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
            "ResizeMode": 0/1/2,
            "MixedVideoLayout":  1,
            "PositionConfig":[
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 0,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                },
                {
                    "RtcStreamId": "user_type",
                    "Region": {
                        "X": 960,
                        "Y": 270,
                        "Z": 1,
                        "Width": 960,
                        "Height": 540
                    }
                }
            ],
            "LayoutConfig": [
                [
                    {
                        "Id": "1_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1",
                            "Height": "1"
                        }
                    }
                ],
                [
                    {
                        "Id": "2_1",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "0",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/1"
                        }
                    },
                    {
                        "Id": "2_2",
                        "Shape": "rectangle",
                        "Area": {
                            "Left": "1/2",
                            "Top": "0",
                            "Width": "1/2",
                            "Height": "1/1"
                        }
                    }
                ]
            ],
            "WaterMark": {
                "Type": 0,
                "Image": "",
                "Text": "",
                "X": 10,
                "Y": 10,
                "Alpha": 0.1
            }
        },

        "SubscribeConfig": {
            "VideoStreamType": 0,
            "MaxSubscriptions": 32,
            "SubscribeAudio": [],
            "UnsubscribeAudio": [],
            "SubscribeVideo": [],
            "UnsubscribeVideo": []
        },

        "RecordingConfig": {
            "MediaChannel": 2,
            "FileType": ["mp4"],
            "StorageConfig": {
                "Vendor": "ucloud",
                "PublicKey": "TOKEN_xxxxx-xxxx-xxxxx-xxxx",
                "SecretKey": "xxxxx-xxxxx-xxxxx-xxxx",
                "Region": "cn-bj",
                "Bucket": "bkc",
                "FileNamePrefix": "/data/mp4/$Jobid.mp4"
            }
        }
    }
}
```
字段具体标识请阅读配置参数详解。

### Internal 配置

参数     | 类型  | 性质 | 描述
---  | --- | --- | ---
JobId       | string类型    | 必填  | 需要转码合成的单流任务的标识。
AppId       | string类型    | 必填  | 开发者ID。
RoomId      | string类型    | 必填  | 房间的ID。
Mode        | int类型       | 必填  | 任务模式，0 单流录制模式，1 合流模式。
ChannelType | int类型       | 必填  | 0 会议模式（小班课）,1 直播模式（大班课）。
RequestId   | string类型    | 必填  | 请求的标识ID

 离线参数请参考[11. 配置参数详解](#)。

## 13. 更新消息通知服务

### 描述
 RESTful API 提供消息通知服务，用户可以指定模块消息通知到用户的消息服务器地址。

### 13.1 开启消息通知
#### 请求

 - Notify结构参数描述

参数     | 类型  | 性质 | 描述
 --  | --- | --- | ---
NotifyUrl       | string类型    | 必填  | 指定用户的消息服务地址。
ServiceType     | string类型    | 必填  | 指定模块的事件通知， 单流录制模式(`job.record`)、云端合流录制模式(`job.mixer`)和传输模块(`job.oss`)。
Status          | string类型    | 必填  | 表示要更新的状态  open 开启 close 关闭

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
                "ServiceType": "job.record",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
        	},
            {
                "Status": "open",
                "ServiceType": "job.oss",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
        	}
        ]
 }
 ```
 
 #### 返回
 
 ```json
 {
     "Version": "1.0",
     "Ack":"job.notify.stat",
     "RetCode": 0,
     "Message": "OK",
     "Internal": {
         "JobId": "xxx"
     },
     "Data": {****
         "NotifyConfig": [
             {
                "Status": "open",
                "ServiceType": "job.record",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
            },
            {
                "Status": "open",
                "ServiceType": "job.oss",
                "NotifyUrl": "http://127.0.0.1:123/callback",
                "Token": "xxxx"
            }
        ]
 }
 ```


## 14. 回调通知
### 描述
  RESTful API 提供消息通知服务，用户可以配置一个接收回调的HTTP/HTTPS 服务器地址来接收云端录制的事件通知，当有事件需要回调通知时，云端录制服务器会将事件通知到云端录制服务的消息服务器，然后云端录制服务通过HTTP/HTTPS 请求的方式将事件投递给用户的消息服务器。
### 14.1 通知请求

   ```json
  {
      "Version": "1.0",
      "Token": "xxxxxxx",
      "ServiceType": "job.record",
      "EventType": "ucloud_record_error",
      "Internal": {
          "JobId": "xxx",
          "RequestId": "0"
      },
      "Data": {
        "ErrorCode": 0,
        "ErrorMsg": "record started successfully"
     }
  ```

  参数     | 类型  | 性质 | 描述
 --  | --- | --- | ---
Version       | string类型    | 选填  | 版本号。
Token         | string类型    | 选填  | 用户服务的鉴权。
ServiceType   | string类型    | 必填  | 指定模块的事件通知， 单流录制模式(`job.record`),直播模式(`job.living`),云端合流录制模式(`job.mixer`)和传输模块(`job.oss`)。
EventType     | string类型    | 必填  | Restful服务通知的事件类型。
Internal      | json类型      | 必填  | 标识某个任务发出的消息。
ErrorCode     | int类型       | 必填  | Restful服务通知的错误码。
ErrorMsg      | string类型    | 必填  | Restful服务通知的事件提示。


Data 字段中的ServiceType, EventType 为请求包体重公共字段，所有回调重都包含这些字段，公共字段的含义详见 消息回调事件。
  
### 14.2 消息回调事件
  本节详细介绍云端录制每种回调事件对应的 ServiceType 以及 Data 包含的具体字段。

EvenType | ServiceType 		| 事件描述
------ |   -----------   	| ---
ucloud_record_status_update 	| job.record  (单流录制服务)		| 单流录制服务状态发生变化
ucloud_record_error      		| job.record  (单流录制服务)		| 单流录制服务发生错误
ucloud_record_warning    		| job.record  (单流录制服务)		| 单流录制服务发生警告
ucloud_record_file_infos    	| job.record  (单流录制服务)		| 单流录制服务文件生成
ucloud_record_session_failover  | job.record  (单流录制服务)		| 单流录制服务启用高可用
ucloud_mixer_status_update  	| job.mixer   (云端合流录制）	   | 云端录制服务状态发生变化
ucloud_mixer_error     			| job.mixer   (云端合流录制）	   | 云端录制服务发生错误
ucloud_mixer_warning     		| job.mixer   (云端合流录制）	   | 云端录制服务发生告警
ucloud_mixer_stream_update   	| job.mixer   (云端合流录制）	   | 云端录制服务流发生变化
ucloud_mixer_file_infos     	| job.mixer   (云端合流录制）	   | 云端录制服务录制文件生成
ucloud_mixer_session_failover	| job.mixer   (云端合流录制）	   | 云端录制服务启用高可用
ucloud_upload_status   			| job.oss     (上传模块）	    | 录制文件上传云端


### 错误码说明

  - ucloud_record_status_update.
   EventType 表示单流录制服务状态发生变化。
    - Status: string 类型， 云端录制当前状态，请参考模块状态服务码。

  - ucloud_record_warning.
      EventType 表示单流录制服务状态发生变化
      - WarnCode: int 类型，告警码，根据当前服务模块类型，查看具体模块的告警码。[16.5 告警码](#)。

  - ucloud_record_error
  	 EventType 表示单流录制服务发生错误
    - ErrorCode: int 类型，错误码。根据当前服务模块类型，查看具体模块的错误码[16.3 错误码](#)。
    - ErrorMsg: string 类型，具体的事件信息。

  - ucloud_record_file_infos.
  	 EventType 表示单流录制文件生成并上传，录制过程中如果mp4文件或者webm文件在ufile中存在，会被覆盖。
    - FileList: Array 类型，生成文件索引 ["example1.mp4", "example2.mp4"]。

  - ucloud_session_failover.
  	EventType 表示服务启动高可用。启动高可用之后，服务端崩溃之后会重新开启一个以原始JodId为标识的服务，默认开启。
    - Status: int 类型， 0 启用成功 1 启用失败。
  - ucloud_living_status_update
      EventType 表示直播转推服务状态发生变化
    - Status: int 类型， 云端录制当前状态，请参考模块状态服务码。

  - ucloud_living_warning.
      EventType 表示直播转推服务状态发生变化
      - WarnCode: int 类型，警告码，根据当前服务模块类型，查看具体模块的告警码。[16.4 状态码](#)。

  - ucloud_living_error.
  	 EventType 表示直播转推服务发生错误
    - ErrorCode: int 类型，错误码。根据当前服务模块类型，查看具体模块的错误码[16.3 错误码](#)。
    - ErrorMsg: string 类型，具体的事件信息。

  - ucloud_living_stream_update.
  	 EventType 表示直播转推服务视频流状态发生变化。
    - StreamUid: string类型，表示当前更新流的uid.
    - Status: string 类型，open 表示开始接收 close 表示停止接收
    - TimeStamp: string 类型，当前操作的时间戳。

  - ucloud_mixer_status_update.
      EventType 表示合流录制服务状态发生变化
    - Status: int 类型， 云端录制当前状态，请参考模块状态服务码。

  - ucloud_mixer_warning.
      EventType 表示合流录制服务状态发生变化
      - WarnCode: int 类型，警告码，根据当前服务模块类型，查看具体模块的告警码。[16.4 告警码](#)。

  - ucloud_mixer_error.
  	 EventType 表示合流录制服务发生错误
    - ErrorCode: int 类型，错误码。根据当前服务模块类型，查看具体模块的错误码[16.3 错误码](#)。
    - ErrorMsg: string 类型，具体的事件信息。

  - ucloud_mixer_stream_update.
  	 EventType 表示合流录制服务视频流状态发生变化。
    - StreamUid: string类型，表示当前更新流的uid.
    - Status: string 类型，open 表示开始接收 close 表示停止接收
    - TimeStamp: string 类型，当前操作的时间戳。


  - ucloud_mixer_file_infos.
  	 EventType 表示合流录制文件生成，录制过程中如果mp4文件或者webm文件在ufile中存在，会被覆盖。
    - FileList: Array 类型，生成文件索引。

  - ucloud_upload_status.
  	 EventType 为31表示上传启动。
    - Status: int 类型，0 表示成功，1 表示上传失败
    - FileList: Array 类型，生成文件索引。


---
### 14.3 错误码
错误码 | 枚举值 		| 描述
------ |   -----------   	| ---
ERROR_OK      						| 0		| 没有错误
ERROR_FAILED       					| 1		| 一般性错误。没有明确归类的错误原因
ERROR_JOB-EXISTENT      		·	| 2		| 任务已开启
ERROR_JOB_NON-EXISTENT      		| 3		| 任务未开启
ERROR_MIXERED_INVALID_VIDEO_PARAM   | 4		| 服务收到无效的视频合流参数
ERROR_MIXERED_INVALID_AUDIO_PARAM  	| 5		| 服务收到无效的音频合流参数
ERROR_MIXERED_STREAM_NON_EXISTENT	| 6		| 服务指定流信息不存在


### 14.4 状态码
#### 服务状态码

状态码  		| 描述
------ |  ---
SERVICE_STATUS_IDEL      		| 服务没有开启
SERVICE_STATUS_RUNNING      	| 服务正在运行
SERVICE_STATUS_EXITING      	| 服务正常退出
SERVICE_STATUS_INTERRUPT    	| 服务中断退出


### 14.5 告警码

告警码 |  描述
------ |  ---
SERVICE_WARN_PROCESS_RESTART    	| 任务异常重启
 
## 15 示例DEMO
  
### 15.1 开启云端录制示例
 
#### 申请资源Id

 ```json
  {
        "Version":"1.0",
        "Action":"job.acquire",
        "Token":"用户token",
        "Internal":{
            "AppId":"xxxxxxx",
            "RoomId":"2234",
            "Mode":1,
            "ChannelType":0,
            "RequestId": "0"
        }
   }
  ```
  
  申请返回的字段，后续操作都得带上这个jobid
  
   ```json
  {
    "Ack" : "job.init",
    "Data" : {
        "JobId" : "xxxxxxx"
    },
    "Internal" : {
        "AppId" : "xxxxxxx",
        "ChannelType" : 0,
        "Mode" : 1,
        "RoomId" : "2234",
        "RequestId": "0"
    },
    "Message" : "OK",
    "RetCode" : 0,
    "Version" : "1.0"
    }
```
  
#### 开启云端录制任务
  
   ```json
 {
 "Version":"1.0",
 "Action":"job.start",
 "Token":"用户token",
 "Internal":{
     "JobId":"URtc-h4r1txxy-2234-1-1613994034",
     "AppId":"URtc-h4r1txxy",
     "RoomId":"2234",
     "Mode":1,
     "ChannelType":0,
     "RequestId": "0"
    },
 "Data":{
     "SubscribeConfig":{
         "VideoStreamType":0,
         "MaxSubscriptions":6,
         "SubscribeAudio":["12342_2","7xhxsbpr_1","a40m7504_1"],
         "SubscribeVideo":["12342_2","7xhxsbpr_1","a40m7504_1"]
    },
    "JobConfig":{
        "IdleTime":60,
        "KeyStream":""
    },
    "TranscodingConfig":{
        "Video":{
            "Codec":"h264",
            "Width":800,
            "Height":600,
            "Fps":15,
            "BitRate":1000,
            "Profile":"highprodile"
        },
        "Audio":{
            "Codec":"aac",
            "SampleRate":48000
        }
    },
    "RecordingConfig": {
        "MediaChannel": 2,
        "FileType": ["mp4"],
        "StorageConfig": {
            "Vendor": "ucloud",
            "PublicKey": "TOKEN_xxxxx-xxxx-xxxxx-xxxx",
            "SecretKey": "xxxxx-xxxxx-xxxxx-xxxx",
            "Region": "cn-xxxxxxx",
            "Bucket": "xxxxxxx",
            "FileNamePrefix": "/data/mp4/$Jobid.mp4"
          }
     },
    }
}
```
  具体参数说明请查询配置参数详解。
  
#### 停止云端录制

```json
{
    "Version": "1.0",
    "Action":"job.stop",
    "Token": "xxxxxxx",
    "Internal": {
        "JobId": "xxxxxxx",
        "RequestId": "0"
    },
    "Data": {}
}

```
具体参数说明请查询配置参数详解。

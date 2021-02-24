# 旁路推流 RESTful 调用方法

使用 RESTful API，无需集成 SDK，通过自己的业务服务器，请求 开启/关闭 旁路推流、更新 旁路推流 的参数。

 - 开始/关闭 旁路推流
 - 查询当前的 旁路推流
 - 更新 旁路推流的流内容
 - 更新 合流配置


## 1. URTC RESTful API数据格式

### 1.1 请求的公共字段

-  RESTful API 使用POST接口类型。

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
Action| 请求的类型，如开启任务、关闭任务，全部类型参看RESTful API [2.1 接口列表](urtc/cdnSteaming/cdnSteaming_RESTfulAPI?id=_21-接口列表)。
Token| 鉴权Token，生成规则参考 [Token生成指导](urtc/sdk/token)。
Internal| 不同Action需要携带的与频道、房间等配置有关的参数。
Data| 不同Action需要携带的跟转码、合流、转推等配置有关的参数。


## 1.2 返回中的公共字段

-  RESTful API 的http返回值永远是HTTP 200，所以不能根据HTTP 返回值判断指令是否成功，需要解析http body中的json内容判断指令是否成功。

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
Ack| string类型|如开启任务、关闭任务，全部类型参看RESTful API [2.1 接口列表](urtc/cdnSteaming/cdnSteaming_RESTfulAPI?id=_21-接口列表)。
RetCode|int类型|错误代码，0 成功，非零代表失败，具体错误代码请参考错误代码总结。
Message|string类型|错误的文本提示。
Internal|json对象|不同Action需要携带的与频道、房间等配置有关的参数。
Data|json对象|根据不同的请求类型，data中的内容也不同。<br>包含着具体请求结果的私有数据。

## 2. 旁路推流 调用时序

下图为实现 旁路推流 需要调用的 API 时序图。    

![](/images/cdnSteamingImage/cdnSteamingStartV2.png)

>查询、更新流、更新合流配置都是可选的，且可以多次调用。但是必须在旁路推流过程中（开始旁路推流后到结束旁路推流前）调用。

## 3. 获取云端资源

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
        "Mode": 1,
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
        "Mode": 1,
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

#### TranscodingConfig：转码配置

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


#### MixerConfig：合流配置

- MaxResolutionStream：string类型，指定合流模板中,最大分辨率的子画面的用户ID及媒体流的类型，`“$userId_$mediaType”`。
    - userId：string类型， 是用户Id。
    - mediaType：int类型，是指摄像头流或桌面流，1 代表摄像头流，2 代表桌面流。
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

#### LiveConfig: 转推配置

- Type： string类型， 转推的协议类型，目前只支持 rtmp 。
- Url: string类型， 转推服务器的地址。

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
        "Mode": 1,
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


## 5. 查询任务状态

### 5.1 查询任务状态的请求
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

## 5.2 查询任务状态的返回
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
        "Mode": 1,
        "ChannelType": 0/1,
    },
    "Data": {
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
            "MaxResolutionStream": “$userId_$mediaType”,
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
            "Crop": false,
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


## 6. 更新流

### 6.1 更新流的请求

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
            "CmdType":1/2/3,更新的动作：1 增加流 2 删除流 3 mute/unmute流
            "SubScribeId": “$userId_$mediaType”
            "HasVideo": true,
            "HasAudio": true,
            "MuteVideo": false,
            "MuteAudio": false
    	}
    }
}
```

#### stream：更新流
 
 - CmdType: string类型，更新的动作： 1 增加流 2 删除流 3 mute/unmute流
 - SubScribeId: string类型，这路流的标识 “$userId_$mediaType”
    - userId：string类型， 是用户Id。
    - mediaType：int类型，是指摄像头流或桌面流，1 代表摄像头流，2 代表桌面流。
 - HasVideo: bool类型，是否有视频
 - HasAudio: bool类型，是否有音频
 - MuteVideo: bool类型，当前视频的状态，true是mute，false是unmute
 - MuteAudio: bool类型，当前音频的状态，true是mute，false是unmute

### 6.2 更新流的响应

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
            "CmdType":1/2/3, 
            "SubScribeId": “$userId_$mediaType”
            "HasVideo": true,
            "HasAudio": true,
            "MuteVideo": false,
            "MuteAudio": false
    	}
    }
}
```

## 7. 更新合流布局

### 7.1 更新合流布局的请求

修改 MixerConfig 合流布局的样式，修改 MixedVideoLayout 布局样式。

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
            "MaxResolutionStream": “$userId_$mediaType”,
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
            "Crop": false,
            "ResizeMode": 2,
            "MixedVideoLayout": 2,修改 MixedVideoLayout 布局样式为2，表示 垂直风格
        }
    }
}
```

### 7.2 更新合流布局的返回

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
        "Mode": 1,
        "ChannelType": 0/1,
    },
    "Data": {
        "MixerConfig": {
            "MaxResolutionStream": “$userId_$mediaType”,
            "BackgroundColor": {"R": 0, "G": 0, "B": 0},
            "Crop": false,
            "ResizeMode": 2,
            "MixedVideoLayout": 2,
        }
    }
}
```

## 8. 停止旁路推流任务

### 8.1 停止旁路推流任务的请求

当房间内无用户，超过预设时间（默认为 60 秒） 后，也会自动停止旁路推流。

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

### 8.2 停止旁路推流任务的返回

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
        "Mode": 1,
        "ChannelType": 0/1,
    },
    "Data": { }
}
```


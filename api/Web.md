# URTC Web SDK API指南

在Web网页浏览器中调用 URTC Web SDK API 可以建立连接，实现实时音视频通话、互动直播服务。     
URTC支持所有的主流浏览器，详见[Web兼容性](/urtc/sdk/VideoStart)。    
[点击这里](https://github.com/ucloud/urtc-sdk-web)，直接下载GitHub仓库的Web SDK。    

URTC Web SDK API包含以下方法：

| 方法 | 描述 |
| -| -|
| [Client 类](https://github.com/ucloud/urtc-sdk-web#client) | 创建客户端 |
| [getDevices 方法](https://github.com/ucloud/urtc-sdk-web#getdevices) | 获取浏览器的音视频设备信息  |
| [getSupportProfileNames 方法](https://github.com/ucloud/urtc-sdk-web#getsupportprofilenames)  | 获取当前 SDK 支持的视频质量的名称  |
| [isSupportWebRTC 方法](https://github.com/ucloud/urtc-sdk-web#issupportwebrtc) | 检测浏览器是否支持`WebRTC` |
| [deviceDetection 方法](https://github.com/ucloud/urtc-sdk-web#devicedetection)  | 发布或预览前的设备检测  |
| [version 属性](https://github.com/ucloud/urtc-sdk-web#version)  |  显示当前 sdk 的版本 |
| [generateToken 方法](https://github.com/ucloud/urtc-sdk-web#generateToken) |  试用时替代服务器生成sdk所需`Token`的方法 |
| [Logger 对象](https://github.com/ucloud/urtc-sdk-web#logger)  | 调试时打印内部日志  |
| [setServers 方法](https://github.com/ucloud/urtc-sdk-web#setservers) | 配置 URTC 服务的域名  |

## Client 类

Client 类包含以下方法：    

| 方法 | 描述 |
| -| -|
|[创建客户端 ](https://github.com/ucloud/urtc-sdk-web#client-constructor) | 构造函数 | 
|[joinRoom 方法 ](https://github.com/ucloud/urtc-sdk-web#client-joinroom) | 加入房间 | 
|[setRole 方法 ](https://github.com/ucloud/urtc-sdk-web#client-setrole) | 设置用户角色 | 
|[leaveRoom 方法 ](https://github.com/ucloud/urtc-sdk-web#client-leaveroom) | 离开房间 | 
|[publish 方法 ](https://github.com/ucloud/urtc-sdk-web#client-publish) | 发布本地流 | 
|[unpublish 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unpublish) | 取消发布本地流 | 
|[subscribe 方法 ](https://github.com/ucloud/urtc-sdk-web#client-subscribe) | 订阅远端流 | 
|[unsubscribe 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unsubscribe) | 取消订阅远端流 | 
|[on 方法 ](https://github.com/ucloud/urtc-sdk-web#client-on) | 绑定事件处理函数 | 
|[off 方法 ](https://github.com/ucloud/urtc-sdk-web#client-off) | 解绑事件处理函数 | 
|[muteAudio 方法 ](https://github.com/ucloud/urtc-sdk-web#client-muteaudio) | 禁用音频轨道 | 
|[unmuteAudio 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unmuteaudio) | 启用音频轨道 | 
|[muteVideo 方法 ](https://github.com/ucloud/urtc-sdk-web#client-mutevideo) | 禁用视频轨道 | 
|[unmuteVideo 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unmutevideo) | 启用视频轨道 | 
|[getUser 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getuser) | 获取当前用户信息 | 
|[getUsers 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getusers) | 获取所有远端用户信息 | 
|[getStream 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getstream) | 获取某条流信息 | 
|[getLocalStreams 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getlocalstreams) | 获取所有本地流信息 | 
|[getRemoteStreams 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getremotestreams) | 获取所有远端流信息 | 
|[getMediaStream 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getmediastream) | 获取某条流对应的媒体流 | 
|[getMicrophones 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getmicrophones) | 获取麦克风设备信息 | 
|[getCameras 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getcameras) | 获取摄像头设备信息 | 
|[getLoudspeakers 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getloudspeakers) | 获取扬声器设备信息 | 
|[setVideoProfile 方法 ](https://github.com/ucloud/urtc-sdk-web#client-setvideoprofile) | 设置视频属性 | 
|[switchDevice 方法 ](https://github.com/ucloud/urtc-sdk-web#client-switchdevice) | 切换音视频输入设备 | 
|[switchScreen 方法 ](https://github.com/ucloud/urtc-sdk-web#client-switchscreen) | 切换屏幕共享 | 
|[switchImage 方法 ](https://github.com/ucloud/urtc-sdk-web#client-switchimage) | 切换图片推送 | 
|[getAudioVolume 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getaudiovolume) | 获取音频音量 | 
|[setAudioVolume 方法 ](https://github.com/ucloud/urtc-sdk-web#client-setaudiovolume) | 设置音频音量 | 
|[getAudioStats 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getaudiostats) | 获取音频状态 | 
|[getVideoStats 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getvideostats) | 获取视频状态 | 
|[getNetworkStats 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getnetworkstats) | 获取网络状态 | 
|[preloadEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-preloadeffect) | 预加载音效文件 | 
|[unloadEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unloadeffect) | 卸载音效文件 | 
|[playEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-playeffect) | 开始播放音效 | 
|[pauseEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-pauseeffect) | 暂停播放音效 | 
|[resumeEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-resumeeffect) | 恢复播放音效 | 
|[stopEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-stopeffect) | 停止播放音效 | 
|[setEffectVolume 方法 ](https://github.com/ucloud/urtc-sdk-web#client-seteffectvolume) | 设置播放音效的音量 | 
|[snapshot 方法 ](https://github.com/ucloud/urtc-sdk-web#client-snapshot) | 截屏 | 
|[startPreviewing 方法 ](https://github.com/ucloud/urtc-sdk-web#client-startpreviewing) | 开启预览 | 
|[stopPreviewing 方法 ](https://github.com/ucloud/urtc-sdk-web#client-stoppreviewing) | 停止预览 | 
|[replaceTrack 方法 ](https://github.com/ucloud/urtc-sdk-web#client-replacetrack) | 替换音频轨道或视频轨道 | 
|[startRecord 方法 ](https://github.com/ucloud/urtc-sdk-web#client-startrecord) |开启录制 |
|[stopRecord 方法 ](https://github.com/ucloud/urtc-sdk-web#client-stoprecord) |结束录制 |
|[updateRecordStreams 方法 ](https://github.com/ucloud/urtc-sdk-web#client-updaterecordstreams) |增加/删除录制的流 |
|[startRelay 方法 ](https://github.com/ucloud/urtc-sdk-web#client-startrelay) |开启旁路推流 |
|[stopRelay 方法 ](https://github.com/ucloud/urtc-sdk-web#client-stoprelay) |结束旁路推流 |
|[updateRelayStreams 方法 ](https://github.com/ucloud/urtc-sdk-web#client-updaterelaystreams) |增加/删除旁路推流的流 |

| 方法 | 描述 |
| -| -|
|[创建客户端 ](https://github.com/ucloud/urtc-sdk-web#client-constructor) | 构造函数 | 
|[joinRoom 方法 ](https://github.com/ucloud/urtc-sdk-web#client-joinroom) | 加入房间 |
|[leaveRoom 方法 ](https://github.com/ucloud/urtc-sdk-web#client-leaveroom) | 离开房间 |
|[publish 方法 ](https://github.com/ucloud/urtc-sdk-web#client-publish) | 发布本地流 |
|[unpublish 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unpublish) | 取消发布本地流 |
|[subscribe 方法 ](https://github.com/ucloud/urtc-sdk-web#client-subscribe) | 订阅远端流 |
|[unsubscribe 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unsubscribe) | 取消订阅远端流 |
|[on 方法 ](https://github.com/ucloud/urtc-sdk-web#client-on) | 绑定事件处理函数 |
|[off 方法 ](https://github.com/ucloud/urtc-sdk-web#client-off) | 解绑事件处理函数 |
|[play 方法 ](https://github.com/ucloud/urtc-sdk-web#client-play) | 播放一条流的音视频 |
|[resume 方法 ](https://github.com/ucloud/urtc-sdk-web#client-resume) | 恢复一条流的音视频播放 |
|[stop 方法 ](https://github.com/ucloud/urtc-sdk-web#client-stop) | 停止播放一条流的音视频 |
|[muteAudio 方法 ](https://github.com/ucloud/urtc-sdk-web#client-muteaudio) | 禁用音频轨道 |
|[unmuteAudio 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unmuteaudio) | 启用音频轨道 |
|[muteVideo 方法 ](https://github.com/ucloud/urtc-sdk-web#client-mutevideo) | 禁用视频轨道 |
|[unmuteVideo 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unmutevideo) | 启用视频轨道 |
|[getUser 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getuser) | 获取当前用户信息 |
|[getUsers 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getusers) | 获取所有远端用户信息 |
|[getStream 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getstream) | 获取某条流信息 |
|[getLocalStreams 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getlocalstreams) | 获取所有本地流信息 |
|[getRemoteStreams 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getremotestreams) | 获取所有远端流信息 |
|[getMediaStream 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getmediastream) | 获取某条流对应的媒体流 |
|[getMicrophones 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getmicrophones) | 获取麦克风设备信息 |
|[getCameras 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getcameras) | 获取摄像头设备信息 |
|[getLoudspeakers 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getloudspeakers) | 获取扬声器设备信息 |
|[setVideoProfile 方法 ](https://github.com/ucloud/urtc-sdk-web#client-setvideoprofile) | 设置视频属性 |
|[switchDevice 方法 ](https://github.com/ucloud/urtc-sdk-web#client-switchdevice) | 切换音视频输入设备 |
|[switchScreen 方法 ](https://github.com/ucloud/urtc-sdk-web#client-switchscreen) | 切换屏幕共享 |
|[switchImage 方法 ](https://github.com/ucloud/urtc-sdk-web#client-switchimage) | 切换图片推送 |
|[getAudioVolume 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getaudiovolume) | 获取音频音量 |
|[setAudioVolume 方法 ](https://github.com/ucloud/urtc-sdk-web#client-setaudiovolume) | 设置音频音量 |
|[getAudioStats 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getaudiostats) | 获取音频状态 |
|[getVideoStats 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getvideostats) | 获取视频状态 |
|[getNetworkStats 方法 ](https://github.com/ucloud/urtc-sdk-web#client-getnetworkstats) | 获取网络状态 |
|[preloadEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-preloadeffect) | 预加载音效文件 |
|[unloadEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unloadeffect) | 卸载音效文件 |
|[playEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-playeffect) | 开始播放音效 |
|[pauseEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-pauseeffect) | 暂停播放音效 |
|[resumeEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-resumeeffect) | 恢复播放音效 |
|[stopEffect 方法 ](https://github.com/ucloud/urtc-sdk-web#client-stopeffect) | 停止播放音效 |
|[setEffectVolume 方法 ](https://github.com/ucloud/urtc-sdk-web#client-seteffectvolume) | 设置播放音效的音量 |
|[snapshot 方法 ](https://github.com/ucloud/urtc-sdk-web#client-snapshot) | 截屏 |
|[startPreviewing 方法 ](https://github.com/ucloud/urtc-sdk-web#client-startpreviewing) | 开启预览 |
|[stopPreviewing 方法 ](https://github.com/ucloud/urtc-sdk-web#client-stoppreviewing) | 停止预览 |
|[~~deviceDetection 方法 ](https://github.com/ucloud/urtc-sdk-web#client-devicedetection) | 已转移~~ |
|[replaceTrack 方法 ](https://github.com/ucloud/urtc-sdk-web#client-replacetrack) | 替换音频轨道或视频轨道 |
|[queryMix 方法 ](https://github.com/ucloud/urtc-sdk-web#client-querymix) | 查询房间内录制或转推任务 |
|[setRole 方法 ](https://github.com/ucloud/urtc-sdk-web#client-setrole) | 设置用户角色 |
|[startRecord 方法 ](https://github.com/ucloud/urtc-sdk-web#client-startrecord) | 开启录制 |
|[stopRecord 方法 ](https://github.com/ucloud/urtc-sdk-web#client-stoprecord) | 结束录制 |
|[updateRecordStreams 方法 ](https://github.com/ucloud/urtc-sdk-web#client-updaterecordstreams) | 增加/删除录制的流 |
|[startRelay 方法 ](https://github.com/ucloud/urtc-sdk-web#client-startrelay) | 开启转推 |
|[stopRelay 方法 ](https://github.com/ucloud/urtc-sdk-web#client-stoprelay) | 结束转推 |
|[updateRelayStreams 方法 ](https://github.com/ucloud/urtc-sdk-web#client-updaterelaystreams) | 增加/删除转推的流 |
|[createStream 方法 ](https://github.com/ucloud/urtc-sdk-web#client-createstream) | 创建一条本地流 |
|[publishStream 方法 ](https://github.com/ucloud/urtc-sdk-web#client-publishstream) | 发布一条本地流 |
|[unpublishStream 方法 ](https://github.com/ucloud/urtc-sdk-web#client-unpublishstream) | 取消发布一条本地流 |
|[destroyStream 方法 ](https://github.com/ucloud/urtc-sdk-web#client-destroystream) | 销毁一条本地流 |



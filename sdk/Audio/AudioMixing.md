# 播放混音

在音视频通话中，除了说话的声音，有时候需要播放自定义的音源、音乐文件让房间内的其他用户听到，比如播放背景音乐等。

<!-- tabs:start -->

# ** Web **

可以播放本地或者在线音乐文件到房间，分享音频或者作为背景音乐，给房间内的其他人。    
Web提供以下两种方法可以满足 播放音乐、播放混音音效的需求。


## 1. 播放音乐

播放音乐是指用户分享本地播放器或者Web浏览器播放的音频，让房间内的其用户听到此音频。    
此方法主要用来播放比较长的背景音，比如直播的时播放的背景音乐，同时只允许有一个文件播放，可以作为暖场音乐、配合 [**屏幕共享**](/video/urtc/sdk/Video/screenshare) 分享声音使用。    
替换发布流的音频轨道或视频轨道，可在保持发布流的发布状态下，切换音频或视频。   
> 播放音乐，会替换本地麦克风的声音。

```js
client.replaceTrack({
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  track: MediaStreamTrack   // 必填，需要替换的新的音轨或视轨，MediaStreamTrack 参见API文档注释
  retain?: boolean    // 选填，是否需要保持被替换的老的音轨或视轨可用，一般情况下，如果后面需要切换回老的音轨或视轨，建议保持其可用，否则可不用保持
}, function(err, oldTrack){
        if(err){
                //执行失败
        }else{
                //执行成功
        }
        //OldTrack 为返回值，MediaStreamTrack 类型，不为空时，值为被替换的音频轨道或视频轨道
})
```
## 2. 播放混音音效

播放混音音效文件方法主要用来播放较短的音效。比如一首歌曲、一段特殊声音效果等。       
音效文件不应过大，否则可能会影响通信的流畅性，支持mp3、aac格式的文件；可以多个音效叠加播放，建议预加载音效文件，可以以提高性能。    
音效由音频文件路径指定可以是本地或网络文件，EffectId 为自行设定的音效ID，需保证唯一性。当音效文件为其他站点的网络文件时，可能会有跨域访问问题。    
播放混音音效，与本地麦克风不冲突，可以同时发布。

### 2.1  预加载音效资源

```js
client.preloadEffect(1, './sounds/liveStreamingOff.mp3', function(err){
        if(err){
                //执行失败
        }else{
                //执行成功
        }
})
```

### 2.2  播放音效
```js
client.playEffect({
          streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
          effectId: number    // 必填，音效资源 ID
          filePath?: string   // 选填，音效文件的路径，当音效文件已经使用 preloadEffect 进行预加载后，可不填此项
          loop?: boolean      // 选填，是否循环播放音效，默认不循环
          playTime?: number   // 选填，音效从 playTime 秒处开始播放，默认为0，即从头开始
          replace?: boolean   // 选填，是否替换当前音轨，即只使用音效，不混音，默认不替换
}, function(err) {
        if(err){
                //执行失败
        }else{
                //执行成功
        }
})
```

### 2.3  暂停播放音效
```js
client.pauseEffect({
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  effectId: number    // 必填，音效资源 ID
}, function(err) {
        if(err){
                //执行失败
        }else{
                //执行成功
        }
})
```
### 2.4  恢复播放音效
```js
client.resumeEffect({
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  effectId: number    // 必填，音效资源 ID
}, function(err) {
        if(err){
                //执行失败
        }else{
                //执行成功
        }
})
```
### 2.5  停止播放音效
```js
client.stopEffect({
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  effectId: number    // 必填，音效资源 ID
}, function(err) {
        if(err){
                //执行失败
        }else{
                //执行成功
        }
})
```
### 2.6  设置正在播放的音效的音量大小
```js
client.setEffectVolume({
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  effectId: number    // 必填，音效资源 ID
  volume: number      // 必填，音量大小，取值范围 [0, 100]
}, function(err) {
        if(err){
                //执行失败
        }else{
                //执行成功
        }
})
```

### 2.7  卸载音效资源
```js
client.unloadEffect(1)

```

# ** Windows **

可以播放本地音乐文件到房间，分享音频或者作为背景音乐，给房间内的其他人。  

## 播放混音音乐

添加mp3、wav格式音频文件的示例代码：    

```cpp
m_rtcengine->startAudioMixing(const char* filepath(本地文件), bool replace（是否取代麦克风输入）, bool loop（是否循环播放）,float musicvol（音乐音量 0.0 -- 1.0）)

```


<!-- tabs:end -->

# 调整通话音量


可以通过SDK获取当前用户的音量，用来做音量检测或者音波图。    
也可以调整 SDK 采集到的声音及 SDK 播放到声卡的声音音量。比如进行双人通话时，想实现静音操作，可以通过调整播放音量的接口将音量设置为 0。    

<!-- tabs:start -->


## ** Web **


### 获取媒体流音量（回调方法）

获取媒体流的音量，返回值范围 [0,100] 
  
示例代码：    
```js
client.getAudioVolume(StreamId);
//StreamId: string 类型，选传，本地或远端流的 ID 即 Stream 的 sid 属性值。当不传时，默认获取第一条本地流的音量大小
```


### 设置媒体流音量

设置媒体流的音量，可设置的音量范围 [0,100] 
  
示例代码：    
```js
client.setAudioVolume({
  streamId?: string,   
  // 选填，发布/订阅流的 ID，不填时，为第一条发布流
  element?: HTMLMediaElement, 
  //仅当为订阅（远端）流，且通过直接将 mediaStream 赋值给 element.srcObject 属性进行播放时必填该 element。若为发布（本地）流，或通过 play 方法进行播放的订阅（远端）流时，不需要填写。
  volume: number 
  // 必填，音量大小，取值范围 [0, 100]
}, function callback(Err) {
  //Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息
});
```


## ** Windows **

### 获取用户音量（回调方法）

balabala……  
  
示例代码：    

balabala……   

### 设置采集音量

balabala……  
  
示例代码：    

balabala……   

### 设置播放音量

balabala……  
  
示例代码：    

balabala……   

### 设置混音音量

balabala……  
  
示例代码：    

balabala……   


# ** Android **

### 获取用户音量（回调方法）

分为本地音量和远端音量
  
示例代码：    

```java
//本地音量获取回调，volume范围0-100
        public void onLocalAudioLevel(int volume) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Log.d(TAG, "Local volume is: " + volume);
                }
            });
        }

//远端音量获取回调，uid表示用户标志，volume范围0-100
        public void onRemoteAudioLevel(String uid, int volume) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Log.d(TAG, "Remote userid: " + uid + " volume is: " + volume);
                }
            });
        }
```  

### 设置采集音量

可以实时调整 SDK 采集到的录音音量。
  
示例代码：    

```java
//参数为0-400的整型类型。
sdkEngine.adjustRecordVolume(volume); 
```

### 设置播放音量

无 
  
示例代码：    

无  

### 设置混音音量

无 
  
示例代码：    

无  


# ** iOS **

### 获取用户音量（回调方法）

balabala……  
  
示例代码：    

balabala……   

### 设置采集音量

balabala……  
  
示例代码：    

balabala……   

### 设置播放音量

balabala……  
  
示例代码：    

balabala……   

### 设置混音音量

balabala……  
  
示例代码：    

balabala……   


<!-- tabs:end -->


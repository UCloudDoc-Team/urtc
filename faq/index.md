# 常见问题

## 加入房间返回的`token`无效

故障原因：  
1. `token` 和 `appid` 不一致，请保证。  
2. `token` 生成规则不对，请参考[Token生成指导](urtc/sdk/token)。  
3. 设置为试用模式，但是没有传入`seckey`。  
4. 设置为正常模式，但是加入房间时没有填写`token` 字段。  

## 集成URTC Web的客户端，无法订阅音视频流

故障原因：  
1. 未部署Web后台服务器。运行本地服务时，Web客户端只能通过本机的 `http://localhost` 或者 `http://127.0.0.1`访问。
2. 部署的是HTTP的Web后台服务器。由于浏览器的安全策略对除`http://localhost` 或者 `http://127.0.0.1`以外的 HTTP 地址作了限制，Web SDK 仅支持 HTTPS 协议，必须使用 HTTPS 访问。

## Chrome谷歌浏览器93版本，URTC SDK无法推拉流
Chrome谷歌浏览器中会有如图的报错信息。    
![ ](images/SDP_PlanB.png)
故障原因：    
Chrome谷歌浏览器自93版本及以后，WebRTC在建立连接阶段将仅支持使用标准的 SDP （Session Description Protocol，是一种通用的会话描述协议）格式：Unified Plan，并彻底废弃 Plan B 语义，影响所有在 Chrome 中使用 Plan B 建立 WebRTC 连接的用户。https://www.chromestatus.com/feature/5823036655665152
解决方案：    
URTC web SDK 1.6.23及以上版本对此变更做了适配。    
URTC web sdk 低于 1.6.23 版本的用户需要升级到最新版本。    

## 无法发布音视频流

故障原因：  
1. 发布音视频，但是设备无麦克风、摄像头设备。
2. 使用了不兼容`webrtc`的浏览器。
3. 客户端未获取到麦克风、摄像头的设备权限，无法发布流。
4. 客户端配置流权限为下行订阅（pull）权限，无法发布流。  
5. 房间内发布流超过发布上限，无法发布流。  

## 发布视频时消息显示发布成功，但是渲染无显示

故障原因：  
1. 摄像头设备被占用，无法发布。  
2. directx驱动不正常，渲染出现问题。  

## 开启录制失败

故障原因：    
1. 控制台上，未开通 录制服务，具体操作参考[开通云端录制](urtc/cloudRecord/index)。 
2. 客户端上，未操作 录制功能，具体操作参考[云端录制SDK代码示例](urtc/cloudRecord/RecordStart)。

## 网络切换时，断开后是否需要调用接口处理

不需要。切换网络时，SDK内部会自动重连，自动连接失败会回调用户，这时用户可以重新加入房间。 

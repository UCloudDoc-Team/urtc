# URTC SDK Web兼容性

Web SDK兼容的浏览器，具体如下表所示：

|平台 | 浏览器 |
|-|-|
|Windows 7+  | Chrome 60+ <br> Firefox 56+ <br> Opera 50+ <br> Edge 浏览器 79+ <br> QQ 浏览器 10+ <br> 360 安全浏览器 10+ <br> 360 极速浏览器 12+  |
|macOS 10+    | Chrome 60+ <br> Firefox 56+ <br> Opera 50+  <br> Edge 浏览器 79+ <br> 360 极速浏览器 1+  <br> 苹果Safari 11+ |
|Android 5.0+  | Chrome 60+ <br> 华为手机浏览器 10+ <br> 微信浏览器 7+ |
|iOS 11+   | 苹果Safari 11+ <br> Chrome 60+（仅支持接收） <br> 微信浏览器 7+（仅支持接收） |
|iOS 14.3+   | 苹果Safari 11+ <br> Chrome 60+  <br> 微信浏览器 7+|

 - iOS 14.3以下的系统有限制，仅允许 苹果safari 浏览器 使用麦克风、摄像头设备；不允许 其他浏览器 使用麦克风、摄像头设备，因此iOS 14.3以下的系统中的微信浏览器、谷歌Chrome浏览器中无法发布视频流，仅支持接收。
 - iOS 14.3及以上的系统放开了设备权限，除了 苹果safari 浏览器，其他浏览器 如 微信浏览器、谷歌Chrome浏览器，可以申请 麦克风、摄像头设备的使用权限，因此iOS 14.3及以上的系统中的 微信浏览器、谷歌Chrome浏览器 可以支持连麦。

 - 移动端浏览器兼容性情况，详见[URTC Web SDK移动端兼容性](urtc/faq/web_mobile)。
 - 其他客户端的兼容性情况，详见 [URTC多平台接入性](urtc/introduction/functions?id=多平台接入) 。













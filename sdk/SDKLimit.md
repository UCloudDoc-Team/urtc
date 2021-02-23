# URTC SDK兼容性

## Web兼容性

URTC Web SDK 兼容的浏览器，具体如下表所示：

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

## Windows兼容性

  - Windows 7 及以上版本的 Windows 系统  
  - Windows SDK 支持如下架构：
	- x86
	- x86-64

## Android兼容性

  - 系统要求：Android 4.1及以上版本的移动设备
  - Android SDK API：等级 16 或以上
  - ABI支持：armeabi-v7a、arm64-v8a

## iOS兼容性

  - Apple设备：iPhone最低支持iPhone5；  
  - 系统版本：最低支持iOS 9.0；  
  - CPU架构：支持arm64、armv7、x86架构，支持真机和模拟器运行；   
  - 其他：v1.5.8及以后版本支持bitcode。
  
## macOS兼容性

  - 系统版本：macOS 10.0；           

## Electron兼容性

  - Windows 7 及以上版本的 Windows 系统  
  - 支持如下架构：
	- x86
	- x86-64

## inux兼容性

  - 系统要求：Linux Ubuntu 16.04、18.04
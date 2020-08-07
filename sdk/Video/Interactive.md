# 互动连麦

在大班课、互动直播的过程中，学生、观众可以连麦，与老师、主播进行互动，丰富课堂、直播体验，提升教学、直播效果。

<!-- tabs:start -->

## ** Web **

### 开始连麦

- 在使用用户权限`role`为 `pull` 的情况下，开始连麦时，要更改用户权限`role`为`push-and-pull`。
- 发布流`publish`，就能与老师、主播互动了。

#### 示例代码

```js

```

### 结束连麦

- 更改用户权限`role`为 `pull`。
- 取消发布流`unpublish`，结束本次连麦。

#### 示例代码

```js

```

### 开发注意事项

balabala

## ** Windows **

### 开始连麦

- 在使用用户权限`setStreamRole`为 `UCLOUD_RTC_USER_STREAM_ROLE_SUB` 的情况下，开始连麦时，要更改用户权限`setStreamRole`为`UCLOUD_RTC_USER_STREAM_ROLE_BOTH`。
- 发布流`publish`，就能与老师、主播互动了。

#### 示例代码

```cpp

```

### 结束连麦

- 在结束连麦时，更改用户权限`setStreamRole`为 `UCLOUD_RTC_USER_STREAM_ROLE_SUB`。
- 取消发布流`unpublish`，结束本次连麦。

#### 示例代码

```cpp

```

### 开发注意事项

balabala

## ** Android **

### 开始连麦

- 在使用用户权限`setStreamRole`为 `UCLOUD_RTC_SDK_STREAM_ROLE_SUB` 的情况下，开始连麦时，更改用户权限`setStreamRole`为`URTC_SDK_STREAM_ROLE_BOTH`。
- 发布流`publish`，就能与老师、主播互动了。

#### 示例代码

```java

```

### 结束连麦

- 在结束连麦时，更改用户权限`setStreamRole`为`URTC_SDK_STREAM_ROLE_BOTH`。
- 取消发布流`unpublish`，结束本次连麦。

#### 示例代码

```java

```

### 开发注意事项

balabala

## ** iOS **

### 开始连麦

- 在使用用户权限`UCloudRtcEngineStreamProfile`为 `UCloudRtcEngine_StreamProfileDownload` 的情况下，开始连麦时，更改用户权限`role`为`UCloudRtcEngine_StreamProfileAll`。
- 发布流`publish`，就能与老师、主播互动了。

#### 示例代码

```objectivec

```

```swift

```

### 结束连麦

- 在结束连麦时，更改用户权限`UCloudRtcEngineStreamProfile`为 `UCloudRtcEngine_StreamProfileDownload`。
- 取消发布流`unPublish`，就结束本次连麦。

#### 示例代码

```objectivec

```

```swift

```
### 开发注意事项

balabala



<!-- tabs:end -->

# 录制回放
URTC 云端录制的音视频文件，会自动存放在配置好的对象存储UFile的存储空间中。录制回放，实际上是播放UFile的录像文件的URL。    
录制任务开始后，会回调当前录制文件的文件名。而由于对象存储UFile的配置不同，录制文件的URL拼接参数不尽相同。


## 获取UFile公开空间的URL地址
| UFile配置 | 录制文件的URL |
|-|-|
|未使用CDN加速| http://"存储空间域名"/"file_name".mp4 |
|使用了CDN加速| http://"CDN加速域名"/"file_name".mp4 |

 - 存储空间域名，可在[UFile](https://console.ucloud.cn/ufile/ufile) 查看。
 - CDN加速域名，可在[UCDN](https://console.ucloud.cn/ucdn/ucdndomainmanage) 查看。
 - file_name，为录制任务开始时，返回的文件名。

## 获取UFile私有空间的URL地址
| UFile配置 | 录制文件的URL |
|-|-|
|未使用CDN加速| http://"存储空间域名"/"file_name".mp4?UCloudPublicKey="UCloudPublicKey"&Expires="Expires"&Signature="Signature" |
|使用了CDN加速| http://"CDN加速域名"/"file_name".mp4?UCloudPublicKey="UCloudPublicKey"&Expires="Expires"&Signature="Signature"  |

 - API公钥`UCloudPublicKey`，可在[UAPI](https://console.ucloud.cn/uapi/apikey) 查看。
 - API有效期`Expires`，用unix时间表示，指URL的超时时间；如果没有这个参数，表示URL永不过期。
 - API签名`Signature`，需要在后台业务服务器生成，具体的生成方法可以参照[UFile的API签名方法](ufile/api/authorization)。

由于UFile的签名方法比较复杂，推荐在后端服务端直接部署UFile SDK。
可以从这里获取[UFile SDK](ufile/tools/sdk)，除了JS SDK其他均可以直接在服务端使用。
使用SDK时，注意要先在配置文件填写`bucket`、`PublicKey`、`PrivateKey`、"存储空间域名"。

通过GetPrivateURL接口，给UFile SDK传入文件名、有效期，即获取私有空间的URL地址。
 
 

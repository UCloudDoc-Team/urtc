{{indexmenu_n>9}}

# Token生成指导

## 1. 流程图

![ ](liuch.png)

## 2. 生成步骤

### 2.1 创建项目并复制AppID及AppKey

具体方法，查看 [快速开始](/video/urtc/quick)。

### 2.2 通过AppId和AppKey 通过sha1算法生成Token

**Token 生成规则**

Token采用类jwt格式：分为头部和数据载荷，形式：header（头部）.signture(数据载荷)。  

#### 2.2.1 生成header

header 部分采用为json 字符串，然后进行base64 编码，json字符串格式如下： 

``` 
Jsonmsg = {
"user_id"： uid，
"room_id"：roomId，
"app_id"： = appId
}
header=base64(Jsonmsg) ;
```

生成base64编码。

```
Headerbase64=base64(jsonmsg) ;
```

#### 2.2.2 生成signature

  -  时间戳获取

时间戳为UTC时间，精确到秒，截取最后10位，作为最终的时间戳。  
伪代码如下： 

``` 
unixts = getutctimes()    
unixts=format(“%10u”, unixts)    
```

  -  随机数生成

随机生成32位的无符号整形数，然后转为16进制，保持8位长度，作为随机数。    
伪代码如下：

``` 
random = random()    
random=format(“%08x”, random)    
```

#### 2.2.3 签名生成

**1. 格式化字符串** 

``` 
strformat = format(“%s%s%d%d%s”, userid, appid, unixts, random, roomid)\\
```


**2. 通过sha1 编码 加密key 为seckey** 

``` 
sign = HmacSign(appCertificate, strformat, HMAC_LENGTH);\\
```


**3. 拼接加密串** 

``` 
signture = format(“%s%d%d”, sign, unixts, random)\\
```

#### 2.2.4 拼接最终的Token

``` 
token = header+ “.”+ signture\\
```

### 2.3 参考实现代码

  - Go 参考代码如下

```
package authcenter
import (
    "crypto/hmac"
    "crypto/sha1"
    "encoding/base64"
    "encoding/hex"
    "encoding/json"
    "errors"
    "fmt"
    "strings"
)

func GenerateRoomToken(uid, appId, roomId, appCertificate string, unixTs int64, randomInt uint32) (string, error) {
    mapCombine := make(map[string]interface{})
    mapCombine["user_id"] = uid
    mapCombine["room_id"] = roomId
    mapCombine["app_id"] = appId
    jsonCombine, err := json.Marshal(mapCombine)
    if err != nil {
        return "", errors.New("SYSTEM_ERROR")
    }

    singnatureStr := Generate(uid, appId, appCertificate, roomId, unixTs, randomInt)
    encodeStr := base64.StdEncoding.EncodeToString([]byte(jsonCombine))
    strCombineToken := encodeStr + "." + singnatureStr

    return strCombineToken, nil
}

func Generate(uId, appID, appCertificate, roomId string, unixTs int64, randomInt uint32) string {
    return generateDynamicKey(uId, appID, appCertificate, roomId, unixTs, randomInt)
}

func generateDynamicKey(uId, appID, appCertificate, roomId string, unixTs int64, randomInt uint32) string {
    unixTsStr := fmt.Sprintf("%010d", unixTs)
    randomIntStr := fmt.Sprintf("%08x", randomInt)
    signature := generateSignature(uId, appID, appCertificate, roomId, unixTsStr, randomIntStr)
    buffer := strings.Join([]string{signature, unixTsStr, randomIntStr}, "")
    return buffer
}

func generateSignature(uId, appID, appCertificate, roomId, unixTsStr, randomIntStr string) string {
    buffer := strings.Join([]string{uId, appID, unixTsStr, randomIntStr, roomId}, "")
    signature := hmac.New(sha1.New, []byte(appCertificate))
    signature.Write([]byte(buffer))
    return hex.EncodeToString(signature.Sum(nil))
}
```

  - Java 参考代码如下

```
import java.io.ByteArrayOutputStream;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;

public class signature {
    // 调用方法生成token
    public static String generateToken(String uid, String roomid, String appid, String seckey){
        long rannum = System.currentTimeMillis()/1000;
        long times = System.currentTimeMillis()/1000;
        String sign = "";
        try {
            sign = generate(uid, appid, seckey,roomid, (int)times, (int)rannum);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return sign ;
    }
    
    public static String generate(String uID,String appID, String appCertificate, String roomID, int unixTs, int randomInt) throws Exception {
        String unixTsStr = ("0000000000" + Integer.toString(unixTs)).substring(Integer.toString(unixTs).length());
        String randomIntStr = ("00000000" + Integer.toHexString(randomInt)).substring(Integer.toHexString(randomInt).length());
        String signature = generateSignature(uID,appID, appCertificate, roomID, unixTsStr, randomIntStr);
        return String.format("%s%s%s", signature, unixTsStr, randomIntStr);
    }

    private static String generateSignature(String uID,String appID, String appCertificate, String roomID, String unixTsStr, String randomIntStr) throws Exception {
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        baos.write(uID.getBytes());
        baos.write(appID.getBytes());
        baos.write(unixTsStr.getBytes());
        baos.write(randomIntStr.getBytes());
        baos.write(roomID.getBytes());
        byte[] sign = encodeHMAC(appCertificate, baos.toByteArray());
        return bytesToHex(sign);
    }

    static byte[] encodeHMAC(String key, byte[] message) throws NoSuchAlgorithmException, InvalidKeyException {
        return encodeHMAC(key.getBytes(), message);
    }

    static byte[] encodeHMAC(byte[] key, byte[] message) throws NoSuchAlgorithmException, InvalidKeyException {
        SecretKeySpec keySpec = new SecretKeySpec(key, "HmacSHA1");

        Mac mac = Mac.getInstance("HmacSHA1");
        mac.init(keySpec);
        return mac.doFinal(message);
    }

    static String bytesToHex(byte[] in) {
        final StringBuilder builder = new StringBuilder();
        for (byte b : in) {
            builder.append(String.format("%02x", b));
        }
        return builder.toString();
    }
}
```

## 3. 申明

Token是SDK验证APP的重要参数，请注意不要明文显示、传输、保存、拷贝。

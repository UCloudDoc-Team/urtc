

# Token生成指导

## 1. Token的使用流程图

![ ](/images/sdk/liuch.png)

## 2. Token生成步骤

### 2.1 创建项目生成AppID及AppKey

创建项目生成AppID及AppKey的具体方法，可以查看 [快速开始](/video/urtc/quick)。

AppKey是保证AppID的秘钥，请妥善保管，避免泄露以及公网明文传输。

### 2.2 sha1算法生成Token

**Token 生成规则**

sha1算法利用AppID及AppKey生成Token，Token采用类jwt格式：分为头部和数据载荷，形式：header（头部）.signture(数据载荷)。  

### 2.3 参考实现代码

  - Go 参考代码如下

```go
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

```java
import java.io.ByteArrayOutputStream;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64 ;

public class AuthToken {
    // 此方法生成token
    public static String generateToken(String uid, String roomid, String appid, String seckey){
        String token = "";
        try {
			String headerjson = "{" + "\"user_id\""+":"+ "\"" +uid  +"\""+","+ "\"room_id\""+":"+ "\""+ roomid+ "\""+","+ "\"app_id\"" +":"+ "\""+ appid + "\""+ "}" ;
		
			final Base64.Encoder encoder = Base64.getEncoder();
			final byte[] textByte = headerjson.getBytes("UTF-8");
			final String base64header = encoder.encodeToString(textByte);
			long rannum = System.currentTimeMillis()/1000;
			long times = System.currentTimeMillis()/1000;
		
            String sign = generate(uid, appid, seckey,roomid, (int)times, (int)rannum);
			token = base64header+"."+sign ;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return token ;
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

 - node js 参考代码如下（需要 base64.js 和  sha1.js ）

```js
import {
    Base64
} from './base64';
import jsSHA from './sha1';

generateToken(obj) {
    const _that = this;
    return new Promise(function (resolve, reject) {
        let header = Base64.encode(JSON.stringify(obj));
        let time = Math.round(Date.now() / 1000);
        let randNum = _that.randNum(8);
        let string = obj.user_id + obj.app_id + time + randNum + obj.room_id;
        let shaObj = new jsSHA("SHA-1", 'TEXT');
        console.log(obj.appkey)
        log(obj.appkey)
        shaObj.setHMACKey(obj.appkey, 'TEXT');
        shaObj.update(string)
        let hash = shaObj.getHMAC("HEX");
        let signture = hash + time + randNum;
        let token = header + '.' + signture;
        // _that.getUrl(token);
        resolve(token);
        if (obj.room_id !== undefined) {
            // resolve('get token success');
        } else {
            reject('get token error')
        }
    })
}
```

```js
generateToken({
    app_id: appId,//控制台创建项目获取到的appkey
    room_id: roomId,//房间号
    user_id: userId,//用户id
    appkey: appkey//控制台创建项目获取到的appkey
}).then(function(data) {
    //返回当前用户的token 
}).catch(function(err){
    //报错信息 
})
```

## 3. 申明

Token是SDK验证APP的重要参数，是用户进入系统的身份验证手段，token 错误将无法进入系统，请确认token 生成代码正确。

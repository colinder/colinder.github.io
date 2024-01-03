# AES vue > django 적용


​	

​	

# AES_vue > django

> <b style="color: #6ae6dc">CryptoJS</b>(vue) 에서 <b style="color: #d99a73">pycryptodomex</b>(django)로 암호화 복호화 하는 과정을 정리

​	

{{< mermaid >}}
graph LR;
    A[Vue] -->|Object data 암호화| B[Django]
    B --> |복호화| B
{{< /mermaid >}}

​	

​	

​	

## ✨기본 사항

1. 각기 다른 시스템의 라이브러리를 사용.
2. AES의 CBC 모드를 기본으로 가정.
3. CBC모드를 사용하기 위해서 필수 인자 3가지
   1. secret_key: 암호화 & 복호화를 위한 메인 키 (대칭키)
   2. iv: 암호화 & 복호화를 위한 서브 키
   3. data: 암호화를 하고 싶은 데이터
4. vue에서 object를 통째로 암호화 하여 django로 전달하는 상황을 가정.
5. <b>💡중요</b>
   - <b style="color: #6ae6dc">CryptoJS</b>
     - secret_key와 iv는 정해진 bytes 길이(long)를 지켜야 한다.  <span style="color: grey; font-size:0.8em">ex) 16bytes, 32bytes 등. 사용자가 길이를 맞추어 사용하기 어렵다면, padding을 사용해 맞춘다.</span>
     - data는 길이가 상관없다. <b style="color: red">CryptoJS는 자동으로 padding하여 bytes길이를 맞추어주기 때문에!</b>
     - 다만 암호화를 하려면 string type의 데이터를 넣어야 한다.
   - <b style="color: #d99a73">pycryptodomex</b>
     - secret_key와 iv, <b style="color: red">data</b>는 정해진 bytes 길이(long)를 지켜야 한다.  <span style="color: grey; font-size:0.8em">ex) 16bytes, 32bytes 등</span> 
     - 이는 이번 포스팅에서는 신경쓸 부분이 아니지만, 나중에 django에서 암호화하여 vue로 보낼때 중요한 사항이다. <span style="color: red; font-size:0.8em">pycryptodomex는 자동으로 data를 padding해주지 않기 때문에!</span> 

​	

​	

​	

​	

## Vue 암호화(encryption)

```javascript
//// import
import CryptoJS from "crypto-js";

//// encryption
var secretKey = 'abcdefghijklmnopqrstuvwxyzabcdef';
var iv = '1234567890123456';
const data = {
    "email": "user12312312@naber.com",
    "password": "1234567890q"
}
var enc = JSON.stringify(data)
const cipher = CryptoJS.AES.encrypt(enc, CryptoJS.enc.Utf8.parse(secretKey), 
    {    
        iv: CryptoJS.enc.Utf8.parse(iv),
        padding: CryptoJS.pad.Pkcs7,  // default setting(없어도 됨)
        mode: CryptoJS.mode.CBC,
    }
)  // out type: Object

this.$axios.post("myServerAdress", { send: cipher.toString() })
```

​			

### 💡암호화 해설

1. ```javascript
   var secretKey = 'abcdefghijklmnopqrstuvwxyzabcdef';
   var iv = '1234567890123456';
   ```

   - 원하는 값으로 변경해서 사용하면 되며, 단 bytes 길이는 맞추어 사용해야 한다. 

2. ```javascript
   const data = {
       "email": "user12312312@naber.com",
       "password": "1234567890q"
   }
   ```

   - Object를 통째로 암호화하여 사용할 것

3. ```javascript
   var enc = JSON.stringify(data)
   ```

   - 암호화를 하려면 string으로 변경해야 해서 변경

4. ```javascript
   const cipher = CryptoJS.AES.encrypt(enc, CryptoJS.enc.Utf8.parse(secretKey), 
       {    
           iv: CryptoJS.enc.Utf8.parse(iv),
           padding: CryptoJS.pad.Pkcs7,  // default setting(없어도 됨)
           mode: CryptoJS.mode.CBC,
       }
   )  // out type: Object
   ```

   - [앞선 포스팅](https://colinder.github.io/aes_vue/)에서 참고 

5. ```javascript
   this.$axios.post("myServerAddress", { send: cipher.toString() })
   ```

   - <b>"myServerAddress"로 post요청을 보내는데 'send'라는 이름으로 암호화한 값을 string type으로 변경하여 전달한다.</b>

​	

​	

​	

## Django 복호화(decryption)

{{< admonition type=info title="info" open=true >}}
django에서 복호화 코드만 정리. api 통신을 위한 내용과 라이브러리들은 정리하지 않는다.
{{< /admonition >}}

```python
import base64
from Cryptodome.Cipher import AES

def decryption(value):  # value == string
    secret_key = 'abcdefghijklmnopqrstuvwxyzabcdef'.encode('utf-8')
    iv = '1234567890123456'.encode('utf-8')
    dec = base64.b64decode(value) # TypeError("Object type %s cannot be passed to C code" % type(data)) 방지
     
    cipher = AES.new(secret_key, AES.MODE_CBC, iv)
    decrypted_data = cipher.decrypt(dec).decode('utf-8')

    # CryptoJS의 자동 padding을 지우고
    # javascript의 Object를 그대로 받아오기 위해
    decrypted_data = decrypted_data.split('}')[0] + "}"
    decrypted_data = eval(decrypted_data)
    return decrypted_data


@permission_classes([AllowAny])
class vue_api(APIView):
    def post(self, request):
        data = request.data['send']
        decrypted_data = decryption(data)
        print(decrypted_data)	# {"email": "user12312312@naber.com", "password": "1234567890q"}
```



​			

### 💡복호화 해설

{{< admonition type=info title="info" open=true >}}
django에서 복호화 코드만 정리. api 통신을 위한 라이브러리들은 정리하지 않는다.
{{< /admonition >}}

1. ```python
   @permission_classes([AllowAny])
   class vue_api(APIView):
       def post(self, request):
           data = request.data['send']
           decrypted_data = decryption(data)
           print(decrypted_data)
   ```

   - API 요청을 접수하여 `request.data['send']`값을 가져오고 이를 `decryption()`함수를 통해 <b>복호화</b> 한다.

2. ```python
   secret_key = 'abcdefghijklmnopqrstuvwxyzabcdef'.encode('utf-8')
   iv = '1234567890123456'.encode('utf-8')
   ```

   - 암호화에 사용된 것과 동일한 secret_key와 iv값을 가져온다.

3. ```python
   dec = base64.b64decode(value) # TypeError("Object type %s cannot be passed to C code" % type(data)) 방지
   ```

   - value(암호화한 데이터)를 base64로 디코딩해준다. 
   - 만약 안해주면 TypeError("Object type %s cannot be passed to C code" % type(data)) 에러가 난다. 
   - 왜 그런건지는 아직 잘 모르겠다.

4. ```python
   cipher = AES.new(secret_key, AES.MODE_CBC, iv)
   decrypted_data = cipher.decrypt(dec).decode('utf-8')
   ```

   - 복호화할 AES를 설정해주고<b>(cipher)</b> cipher의 <b>decrypt()</b>함수를 사용해 복호화 한다. <span style="color: grey; font-size:.8em">> output type은 bytes</span>

   - 복호화한 값은 encoding된 값<span style="color: grey; font-size:.8em">(bytes type)</span>이라서 decode('utf-8')를 사용해 string type으로 변경해준다.

     ​	

   ### 여기서 부터 중요!

   여기까지 진행하고 복호화된 값(decrypted_data)을 출력해보면

   `{"email": "user12312312@naber.com", "password": "1234567890q"}══════`와 같이 <b style="color: #6ae6dc">CryptoJS</b>가 bytes길이를 맞추기 위해 padding된 공백값들이 `══════`같은 특수문자로 채워져있다.

5. ```python
   # CryptoJS의 자동 padding을 지우고 javascript의 Object를 그대로 받아오기 위해
   decrypted_data = decrypted_data.split('}')[0] + "}"
   decrypted_data = eval(decrypted_data)
   ```

   - 하여 Object의 데이터를 그대로 사용하기 위해 "}"를 기준으로 `split()`하고 다시 "}"를 붙여 dictionary 형태의 string으로 만들었다. 
   - 그리고 이를 eval()함수를 사용해 dictionary type으로 만들었다.
   - 아직 정확한 원인?을 찾지 못했습니다.
   - <b style="color: #6ae6dc">CryptoJS</b>와 <b style="color: #d99a73">pycryptodomex</b>라는 라이브러리의 차이 때문에 발생한 원인으로 이해하고 넘어갔습니다.

6. ```python
   return decrypted_data
   ```

   - 그리고 전처리가 다 끝난 값을 return 해 종료

​	

​	

​	

​	

## 마무리

> 이로써 <b style="color: #6ae6dc">CryptoJS</b>(vue) 에서 <b style="color: #d99a73">pycryptodomex</b>(django)로 암호화 복호화 하는 과정 정리를 마무리.
>
> 모든 과정을 정확히 이해하기 위해 공식문서를 다 참고하였지만, 아직 명확하지 않은 부분도 남아있습니다. 
>
> 저는 다른 사람들이 작성한 코드도 많이 보았지만, 가장 정확한 사용방법은 공식문서와 github을 참고해 라이브러리의 코드를 직접 보며 이해하는 것입니다.  다른 분들 그리고 저를 포함해 남들이 작성한 코드에 현혹되지 마세요. 대충 코드가 돌기만하면 된다 하시는 분은 그냥 레퍼런스 많이 보면 이해가 될 것입니다.

​	

​	


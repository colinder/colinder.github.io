# AES django > vue 적용


​	

​	

# AES_django > vue

> <b style="color: #d99a73">pycryptodomex</b>(django)에서 <b style="color: #6ae6dc">CryptoJS</b>(vue)로 암호화 복호화 하는 과정을 정리

​	

{{< mermaid >}}
graph LR;
    A[Django] -->|string data 암호화| B[vue]
    B --> |복호화| B
{{< /mermaid >}}

​	

​	

​	

## ✨기본 사항

1. django에서 6자리 숫자의 OTP를 생성해 암호화 하여 vue로 전달하는 상황을 가정.
2. 각기 다른 시스템의 라이브러리를 사용.
3. AES의 CBC 모드를 기본으로 가정.
3. CBC모드를 사용하기 위해서 필수 인자 3가지
   1. secret_key: 암호화 & 복호화를 위한 메인 키 (대칭키)
   2. iv: 암호화 & 복호화를 위한 서브 키
   3. data: 암호화를 하고 싶은 데이터
5. <b>💡중요</b>
   - <b style="color: #d99a73">pycryptodomex</b> 암호화
     - 해당 라이브러리에서는 bytes type의 데이터만 인자로 받을 수 있다. 하여 encode(), decode() 함수를 사용해 데이터 타입을 변경해 주었다.
     - secret_key와 iv, <b style="color: red">data</b>는 정해진 bytes 길이(long)를 지켜야 한다.  <span style="color: grey; font-size:0.8em">ex) 16bytes, 32bytes 등</span> 
     - <b style="color: red;">pycryptodomex는 자동으로 data를 padding해주지 않는다.</b>
   - <b style="color: #6ae6dc">CryptoJS</b> 복호화
     - secret_key와 iv는 암호화에 사용된 값과 동일해야 한다.

​	

​	

​	

​	

## Django 암호화(encryption)

{{< admonition type=info title="info" open=true >}}
django에서 복호화 코드만 정리. api 통신을 위한 내용과 라이브러리들은 정리하지 않는다.
{{< /admonition >}}

```python
import base64
from Cryptodome.Cipher import AES
from Cryptodome.Util.Padding import pad

def encryption(value):
    secret_key = 'abcdefghijklmnopqrstuvwxyzabcdef'.encode('utf-8')
    iv = '1234567890123456'.encode('utf-8')
    
    # pad를 위해서 bytes로 변환 필요 
    # 최소 데이터 길이 == 16bytes 로 만들어 주기 위해 pad 진행
    data = value.encode('utf-8')
    # Data must be padded to 16 byte boundary in CBC mode
    padded_data = pad(data, 16)
    
    cipher = AES.new(secret_key, AES.MODE_CBC, iv)
    encrypted_data = base64.b64encode(cipher.encrypt(padded_data))
    return encrypted_data  # bytes type 


@permission_classes([AllowAny])
class Otp(APIView):
    def get(self, request):
        OTP = ""
        for _ in range(6):
            temp = random.randrange(0, 10)
            OTP += str(temp)        

        response = {
            "detail": "요청하신 사항이 처리 되었습니다.", 
            "OTP": encryption(OTP)  # <class 'bytes'>
        }
        return Response(response)
```



​			

### 💡암호화 해설

1. ```python
   @permission_classes([AllowAny])
   class Otp(APIView):
       def get(self, request):
           OTP = ""
           for _ in range(6):
               temp = random.randrange(0, 10)
               OTP += str(temp)        
   
           response = {
               "detail": "요청하신 사항이 처리 되었습니다.", 
               "OTP": encryption(OTP)  # <class 'bytes'>
           }
   ```

   - get요청을 받으면, 6자리 숫자 string type의 OTP를 생성하고 이를 암호화(<span style="color: grey; font-size:.8em">encryption()</span>) 한다. 

2. ```python
   secret_key = 'abcdefghijklmnopqrstuvwxyzabcdef'.encode('utf-8')
   iv = '1234567890123456'.encode('utf-8')
   ```

   - 키에 관련된 변수 세팅

3. ```python
   data = value.encode('utf-8')
   padded_data = pad(data, 16)
   ```

   - <b style="color: #d99a73">pycryptodomex</b> 공식문서를 보면 'Data must be padded to 16 byte boundary in CBC mode' 이다.
   - 하여 padding을 주어야 하는데 <b style="color: #d99a73">pycryptodomex</b>의 pad() 함수를 사용한다. 
   - pad() 함수는 bytes type의 데이터만 다룰 수 있어서 .encode('utf-8')로 bytes type으로 변경하고
   - 이를 pad() 함수에 넣어 처리한다. 

4. ```python
   cipher = AES.new(secret_key, AES.MODE_CBC, iv)
   ```

   - 암호화 세팅을 해주고

5. ```python
   encrypted_data = base64.b64encode(cipher.encrypt(padded_data))
   ```

   - bytes type의 변수 'padded_data'를 암호화
   - cipher.encrypt(padded_data)는 `b'8\x08\xa5\x90\xed;Q\x18\x86\xcb\xbf\nY\xd1\x9e\x87'`와 같은 형태
   - base64.b64encode() 함수에 넣어 `b'fGJc4x3IqRkIApLmz/atxw=='`와 같이 변환한다.

​	

​	

​	

​	

## Vue 복호화(decryption)

```javascript
var encrypted_data = res.data.OTP
const decryptde_data = CryptoJS.AES.decrypt(encrypted_data, CryptoJS.enc.Utf8.parse(secretKey), {
    iv: CryptoJS.enc.Utf8.parse(iv),
    padding: CryptoJS.pad.Pkcs7, // default setting(없어도 됨)
    mode: CryptoJS.mode.CBC,
});

console.log('OTP', decryptde_data.toString(CryptoJS.enc.Utf8))
```







​			

### 💡암호화 해설

1. ```javascript
   var encrypted_data = res.data.OTP
   ```

   - 응답온 데이터를 변수로 저장

2. ```javascript
   var secretKey = 'abcdefghijklmnopqrstuvwxyzabcdef';
   var iv = '1234567890123456';
   ```

   - 암호키 설정값 가져오고

3. ```javascript
   const decryptde_data = CryptoJS.AES.decrypt(encrypted_data, CryptoJS.enc.Utf8.parse(secretKey), {
       iv: CryptoJS.enc.Utf8.parse(iv),
       padding: CryptoJS.pad.Pkcs7, // default setting(없어도 됨)
       mode: CryptoJS.mode.CBC,
   });
   ```

   - AES 세팅하고 암호화된 데이터(encrypted_data)를 넣어 복호화 한다.

4. ```javascript
   console.log('OTP', decryptde_data.toString(CryptoJS.enc.Utf8))
   ```

   - type을 변환해주고 console로 확인해본다.

​		

​		

​		

​	

​	

## 마무리

> 이로써 <b style="color: #d99a73">pycryptodomex</b>(django)에서  <b style="color: #6ae6dc">CryptoJS</b>(vue)로 암호화 복호화 하는 과정 정리를 마무리.
>
> 두 라이브러리의 차이를 이해하고, 단계 단계마다 어떻게 데이터가 변하는지 확인하는 것이 좋을 것 같습니다.

​	

​	

​	

​	

​	


# AES django 적용


​	

​	  

# AES django 적용

> django에 [PyCryptodomex](https://pypi.org/project/pycryptodomex/)를 설치하여 적용

```bash
pip install pycryptodomex
```

- 주의 사항 

  1. PyCryptodome is a fork of PyCrypto

  2. For more information, see the [homepage](http://www.pycryptodome.org/).를 클릭하면, <b>pycryptodome</b>의 문서로 이동하는데, 왜 그런건지 모르겠다. 아마 의존성?이 있어 그런건가 싶다. 

  3. 문서에 예시를 보면 

     ```django
     from Crypto.Cipher import AES
     ```

     와 같이 import 하라는데, 이러면 안되고

     ```django
     from Cryptodome.Cipher import AES
     ```

     이렇게 Crypto > Cryptodome 으로 바꿔주어야 사용이 가능하다.

  4. pycryptodome은 오직 bytes로 인코딩 된 데이터만 처리할 수 있다. 

  5. AES를 이용 암호화하려면, 암호화의 대상인 text가 16, 32, 64, 128, 256 바이트의 블록들(데이터)이어야 한다.

  6. 위와 같이 암호화 대상인 text를 16, 32, 64, 128, 256 바이트의 블록들로 만드는 것을 padding이라고 한다.

  7. <b>개인적으로 전체적인 데이터 처리를 위한 타입을 맞춰주는 것이 가장 어려웠다.</b>

  8. Front에서 어떤 데이터를 전달 받은 상황을 가정하고 예시 제작

​	

​	

​	

## 암호화(encryption)

```python
def encryption(value):
    secret_key = 'abcdefghijklmnopqrstuvwxyzabcdef'
    iv = '1234567890123456'		
    SK = secret_key.encode('utf-8') 
    IV = iv.encode('utf-8')
    
    data = value.encode('utf-8')	# pad를 위해서 bytes로 변환 필요 
    paded_data = pad(data, 16)		# 최소 데이터 길이 == 16bytes 로 만들어 주기 위해 pad 진행
    
    cipher = AES.new(SK, AES.MODE_CBC, IV)
    encrypted_data = base64.b64encode(cipher.encrypt(paded_data))
    return encrypted_data  # type bytes
```

​			

### 💡암호화 유의사항

1. ```python
   data = value.encode('utf-8')
   paded_data = pad(data, 16)
   ```

   - pad를 위해서 bytes로 변환 필요 
   - data의 길이가 16보다 작으면, 최소 데이터 길이 == 16bytes 로 만들어 주기 위해 pad 진행

​	

​	

​	

## 복호화(decryption)

```python
def decryption(value):
    secret_key = 'abcdefghijklmnopqrstuvwxyzabcdef'
    iv = '1234567890123456'			
    
    SK = secret_key.encode('utf-8')
    IV = iv.encode('utf-8')
    dec = base64.b64decode(value)
    
    cipher = AES.new(SK, AES.MODE_CBC, IV)
    decrypted_data = cipher.decrypt(dec).decode('utf-8')
    return decrypted_data

data = request.data		# 암호화된 string
result = encryption(data)
```

​		

### 💡복호화 유의사항

1. ```python
   secret_key = 'abcdefghijklmnopqrstuvwxyzabcdef'     # AES256은 key 길이가 32자여야 함
   ```

   - front에서 전달받은 암호화 데이터를 복호화하는 것이니. 암호화때 사용된 secret_key와 동일한 secret_key 정보가 있어야 함.

2. ```python
   iv = '1234567890123456'
   ```

   - iv도 마찬가지

3. ```python
   SK = secret_key.encode('utf-8')
   IV = iv.encode('utf-8')
   ```

   - pycryptodomex는 bytes type의 데이터만 다룰 수 있어 변환
   - 테스트 결과 'utf-8' 이든 'ascii'든 encode 하는 타입은 복호화에 영향이 없었다. (아무거나 선택 가능)
   - 아마도 bytes 로만 변경되면 다룰 수 있기 때문에 가능한 것 같다. 또 utf-8이 ascii보다 더 포괄적인 사용 가능한 인코딩 방식

​	

​	

​	

​	

​	

### 👀알아낸 것

1. ```python
   sent = '8ac9c453ae4f5056'
   a = sent.encode('utf-8')	# b'8ac9c453ae4f5056'
   b = a.decode('utf-8')		# 8ac9c453ae4f5056
   ```

   - .encode() .decode()는 간단히 string type <> bytes type로 변경해주는 함수

     ​	

2. ```python
   sent = b'8ac9c453ae4f5056'
   a = base64.b64encode(sent)	# b'OGFjOWM0NTNhZTRmNTA1Ng=='
   b = base64.b64decode(a)		# b'8ac9c453ae4f5056'
   ```

   - base64.b64encode(), base64.b64decode()는 간단히 btyes type의 데이터를 [base64](https://ko.wikipedia.org/wiki/%EB%B2%A0%EC%9D%B4%EC%8A%A464)로 변경해주는 함수

     ​	

3. 인코딩 
   - 뜻: <b>정보의 형태나 형식을 변환하는 처리나 처리 방식</b>

​	

​	

​	

​	

​	

​	

## ✨주의사항

> 보안 코드 전문가가 아니기 때문에, 구글링을 통하여 테스트해보고 실제 동작이 확인된 사항들만 정리하였습니다. 
>
> 즉, 다른 방법도 있을 것이고 다른 분의 깔끔한 코드도 금방 찾아 볼 수 있으니, 많은 레퍼런스, 많은 테스트를 진행해보길 추천합니다.
>
> 전 type 맞추는 것이 제일 복잡했습니다.
>
> 또 저는 vue와 연동을 하면서 개발하여서, 앞에 vue 정리사항과 같이 보면 더 좋습니다.

- 참고 레퍼런스

  https://inma.tistory.com/145

  https://www.pycryptodome.org/src/examples

​	

​	

​	

​	

​	

​	

​	

​	

​	

​	

​	


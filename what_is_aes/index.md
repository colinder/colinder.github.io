# what is AES


​		

​		

​	

# AES 보안 

> 보안 진단을 받아보니, 생각보다 API 통신은 해킹이 쉽다.
>
> 하여 보안에 신경써야 하는 경우, 기술들이 다양하게 있는데, 그 중 AES<**A**dvanced **E**ncryption **S**tandard>를 간단히 알아보고 이후 적용하는 방법과 과정에 대하여 정리한다. 
>
> AES는 [미국 표준 기술 연구소](https://namu.wiki/w/NIST)에 의해서 연방 정보 처리 표준으로 지정된 암호화 방식이며 [NSA](https://namu.wiki/w/NSA)에 의해 1급 비밀에 사용할 수 있도록 승인된 암호화 알고리즘 중 유일하게 공개된 알고리즘 - 출처 : 나무위키

​	

​	

​	

## 환경 설정

1. frontend = vue.js / backend = django로 API 연동하여 사용.
   - 즉 vue.js에서 암호화한 값을 django에서 복호화해서 사용하고, 
   - django에서 암호화한 값을 vue.js에서 복호화해서 사용하는 환경을 가정.
2. vue.js는 CryptoJS를 사용
3. django는 PyCryptodomex를 사용

​	

​	

​	

## 기본 지식 & 설정

1. AES는 대칭키 암호화 방식이며, front와 back이 공유해야하는 <b>키</b>가 존재한다.

   - secret_key :  비공개로 관리되어야 하며, 암호를 걸거나 풀 때 사용하는 특별한 KEY

     ​	


2. 사용하려는 AES에 디폴트값인 MODE_CBC(Cipher Block Chaining) 방식를 사용하려고 한다. 

   - 블록 암호화 운영 모드 중 **보안 성이 제일 높은** 암호화 방법으로 **가장 많이 사용**된다.
   - **IV(Initial Vector)**라는 값을 설정해주어야하는데, 이는 암호화를 랜덤화하기 위해 여러 모드에서 사용되는 비트 블록이다. 
   - 암호문이 블록의 배수가 되기 때문에 복호화 후 평문을 얻기 위해서 **Padding**을 해야만 한다.

   해당 사항들을 다 이해하고 사용하면 좋겠지만, 우선 iv라는 설정값이 필요하다, padding을 해주어야 한다. 정도만 알고 진행해도 무방하다.

   ​		

3. 즉 AES의 CBC 방식을 사용하기 위해서는

   - secret_key
   - iv
   - padding의 과정

   이 세가지를 생각하며 진행하면 된다.

​	

​		

​			

### 잠시 특이사항 정리

1. secret_key

   - It must be 16 (*AES-128)*, 24 (*AES-192*) or 32 (*AES-256*) bytes long.

     For `MODE_SIV` only, it doubles to 32, 48, or 64 bytes.

     ​		

2. iv

   - The initialization vector is used to ensure distinct [ciphertexts](https://en.wikipedia.org/wiki/Ciphertext) are produced even when the same [plaintext](https://en.wikipedia.org/wiki/Plaintext) is encrypted multiple times independently with the same [key](https://en.wikipedia.org/wiki/Key_(cryptography)). - 출처: [위키피디아](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher_Block_Chaining_.28CBC.29)

     동일한 iv를 사용해도 암호화된 내용은 변경된다고 한다.

   - 그러나 레퍼런스들을 찾아보니 secret_key는 고정된 값이지만, iv는 random하게 생성해서 사용하는 경우가 많은 것 같다. 

   - iv는 For <b>`MODE_CBC`</b>, `MODE_CFB`, and `MODE_OFB` it must be <b>16 bytes long</b>.

     ​	

3. 하여 <b>secret_key</b>(32bytes), <b>iv</b>(16bytes)는 고정된 값으로 생성 후 사용하려고 한다. 

​	

​	

​	

​	

​	



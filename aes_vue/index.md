# AES vue 적용


​	

​	

# AES Vue 적용

> vue.js에 [CryptoJS](https://github.com/brix/crypto-js)를 설치하여 진행

```bash
npm i crypto-js
```

- 주의 사항 
  1. AES에 사용되는 parameters는 모두 bytes 타입만 사용이 가능하다. 
  2. 하여 대부분 utf-8로 encoding하여 사용 (이 부분은 아직 정확하게 이해하지 못했다.)

​					

​	

​	

## 암호화(encryption)

```javascript
var secretKey = 'abcdefghijklmnopqrstuvwxyzabcdef';
var iv = '1234567890123456';
var data = this.accounts.email.padEnd(32, " ")
const cipher = CryptoJS.AES.encrypt(data, CryptoJS.enc.Utf8.parse(secretKey), {
    iv: CryptoJS.enc.Utf8.parse(iv),
    padding: CryptoJS.pad.Pkcs7,  // default setting(없어도 됨)
    mode: CryptoJS.mode.CBC,
});  // out type: Object

this.$axios.post("myServerAdress", { cipher: cipher.toString() })
```

​			

### 💡암호화 유의사항

1. ```javascript
   var secretKey = 'abcdefghijklmnopqrstuvwxyzabcdef';
   ```

   - AES256은 secretKey 길이가 32자여야 함

2. ```javascript
   var iv = '1234567890123456';
   ```

   - mode.CBC모드를 사용하려면 iv 길이는 16자여야 함

3. ```javascript 
   var data = this.accounts.email.padEnd(32, " ")
   ```

   - 전달할 데이터에 최소 32bytes padding을 주지 않으면, 
   - django에서 데이터를 받는데 user@naver.com╗╗ 와 같이 알 수 없는 특수문자가 포함되어서 왔다.

4. ```javascript
   this.$axios.post("myServerAdress", { cipher: cipher.toString() })
   ```

   - params로 담기 위해선 string tpye 변환이 필요 
   - toSting()안하면, Converting circular structure to JSON 에러 발생
     선회하는 구조를 JSON으로 바꾸려고 해서 나는 에러라는데 뭔지 잘 모르겠음.

   - .toString() 함수를 사용하지 않으면, 암호화된 Object 타입이 되는데, 대략 아래와 같은 모습이다. 

     - *{$super: {…}, ciphertext: W…y.init, key: W…y.init, iv: W…y.init, init: \*ƒ*, …}*

       1. **$super**: {$super: {…}, init: *ƒ*, toString: *ƒ*}

       1. **algorithm**: {keySize: 8, _doReset: *ƒ*, encryptBlock: *ƒ*, decryptBlock: *ƒ*, _doCryptBlock: *ƒ*, …}

       1. **blockSize**: 4

       1. **ciphertext**: WordArray.init {words: Array(12), sigBytes: 48}

       1. **formatter**: {stringify: *ƒ*, parse: *ƒ*}

       1. **init**: *ƒ ()*

       1. **iv**: WordArray.init {words: Array(4), sigBytes: 16}

       1. **key**: WordArray.init {words: Array(8), sigBytes: 32}

       1. **mode**: {$super: {…}, Encryptor: {…}, Decryptor: {…}, init: *ƒ*}

       1. **padding**: {pad: *ƒ*, unpad: *ƒ*}

       1. [[Prototype]]: Object

5. - 여기에 .toString() 함수를 붙여주면.
     <b>0zs9xTyPtLkJDvwXCH1G4PZqxUwfuf/LVIq5Ovs3bR39B0sCxl87IxSaE1mtS4RE</b><br>와 같이 암호화가 된 것을 볼 수 있다. 

     ​		

     ​		

     ​	

## 복호화(decryption)

```javascript
var secretKey = 'abcdefghijklmnopqrstuvwxyzabcdef';
var iv = '1234567890123456';
var encrypted_data = res.data.OTP  // 복호화할 데이터 (tpye string)
const decryptde_data = CryptoJS.AES.decrypt(encrypted_data, CryptoJS.enc.Utf8.parse(secretKey), {
    iv: CryptoJS.enc.Utf8.parse(iv),
    padding: CryptoJS.pad.Pkcs7, // default setting (없어도 됨)
    mode: CryptoJS.mode.CBC,
});

console.log('복호화', CryptoJS.enc.Utf8.stringify(decryptde_data).toString())
```

​			

### 💡복호화 유의사항

1. 본 테스트는 django에서 진행하였는데 django에서 전달되는 데이터 형태는 string이다.

2. 암호화에 사용된 동일한 secretKey, iv를 가져와서 사용해야 한다. (대칭키니까 당연하게도)

3. 레퍼런스를 확인해보니 전달되는 데이터에 iv를 포함하여 전달하고 받는 형태도 있던데, 저는 그냥 secretKey, iv를 별도로 지정하여 사용하였습니다.

4. ```javascript
   var encrypted_data = res.data.OTP
   ```

   - django에서 전달 받은 데이터를 변수로 저장. / 대략 <b>wh5woZao84Sgh7NmhQJVvQ==</b> 이런 모습

​	

​	

​	

​	

​	

​	


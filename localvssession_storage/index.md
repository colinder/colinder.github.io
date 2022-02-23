# localStorage vs sessionStorage


​	

# Local Storage와 Session Storage의 차이

를 알아보기 전에 

​		

​	

## 'Storage(web storage)' 란 무엇인지 우선 알아봅시다.

> HTML5에서 등장한 웹의 데이터를 클라이언트에 저장할 수 있는 기능

인터넷상의 통신을 하는데 어떤 자료를 가지고 있어야 한다거나, 나의 정보(유저정보)를 관리하는 등의 **데이터 관리를 위한 저장소 기능.** 그리고 storage는 두 가지 종류가 존재

​			

​			

## Local Storage와 Session Storage

1. Local storage

   origin<span style="color: grey; font-size:0.8em">(요청이 시작된 서버를 나타내는 URL)</span>이 같을 경우, 여러 탭과 브라우저 창에서 공유되는 저장소.  Session<span style="color: grey; font-size:0.8em">(클라이언트와 웹 서버간에 통신 연결에서 두 개체의 활성화된 접속)</span>이 종료된 이후에도 지속(유지)되는 저장소로 설계되어 컴퓨터를 종료하거나 브라우저를 종료하더라도 저장되어있던 데이터는 사라지지 않고 지속하여 보관한다.

   ​					

2. Session storage

   브라우저의 한 탭에서 페이지의 세션이 유지되는 동안 origin별로 storage를 관리하는 저장소. 페이지가 열려 있는 동안이나, 페이지 reloading 혹은 복원 시에는 데이터가 유지되지만, 다른 session이나 탭창이 종료될 경우, 데이터에 접근할 수 없다. 하여 Local storage보다 제한적으로 사용된다.

​				

​	

## 공통점(특징)

1. Object와 비슷한 **key-value**의 구조 <span style="color: grey; font-size:0.8em">ex) {'username': 'imdeveloper', 'age': 'imold'}</span>

2. 모든 key와 value는 **String으로 저장**

​			

​			

## 간단한 사용법

```javascript
// Local Storage

// 1 .Local Storage에 정보 저장하기 - setItem()
localStorage.setItem("name", "value")

// 2. Local Storage에서 정보 가져오기 - getItem()
localStorage.getItem("name")
```

```javascript
// Session Storage

// 1. Session Storage에 정보 저장하기 - setItem()
sessionStorage.setItem("name", "value")

// 2. Session Storage에서 정보 가져오기 - getItem()
sessionStorage.getItem("name")
```

​	

​	

​	

​	



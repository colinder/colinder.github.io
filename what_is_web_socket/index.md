# What is Web Socket?


​	

# What is Web Socket?

>Web 통신은 무조건 두 단계를 걸쳐 동작합니다. 요청과 응답. 간단히 어떤 요청이 있을 때만 서버와 연결되어 어떤 응답을 하게 됩니다. 이러한 방식을 HTTP라고 합니다. 
>예를 들어, Naver.com에 접속하는 것은 'Naver.com'이라는 요청을 보낸 것이고 그에 대한 응답으로 naver의 메인 화면을 보여주는 것이죠.
>그런데 만약 실시간 채팅 서비스나 푸시 알람을 구현해야 한다면, 매우 잦은 요청을 보내야 하고 네트워크 상 많은 부하를 발생시킬 수 있습니다.

HTML5이라는 웹 표준이 생긴이래로 이러한 HTTP의 단점을 보완하기 위해 웹 소켓(Web Socket)이 등장했습니다. 

웹 소켓은 서버와 사용자 간 연결을 유지한 상태로 추가 요청없이 양방향으로 데이터를 교환할 수 있는 프로토콜입니다. 이런 특성은 실시간으로 데이터 교환이 지속해서 일어냐야 하는 서비스에서 유용합니다.

​	

​	

---

## Web Socket 사용법

웹 소켓 연결을 만들 때는 Websocket 생성자를 이용합니다. 이때 매개변수 url에서 사용하는 프로토콜은 ws와 wss입니다. 둘의 관계는 http와 https와의 관계와 비슷하며 상대적으로 wss가 보안과 신회성이 더 높은 프로토콜입니다.

```javascript
const socket = new WebSocket("wss://something_URL.com")
```

웹 소켓의 핸드쉐이크(handshake) 과정은 HTTP를 이용합니다. HTTP는 반드시 1.1버전 이상이어야 하며 GET method를 이용해 요청을 보냅니다. 핸드쉐이크 과정을 간략하게 정리하면 아래와 같습니다.

1. 브라우저에서 서버로 웹 소켓 지원여부를 물어봅니다.

2. 이때 아래 코드와 같이 Connection 헤더와 Upgrade 헤더의 값을 설정합니다.

3. 이 값의 의미는 클라이언트 측에서 프로토콜을 웹 소켓 프로토콜로 변경하고 싶다는 의미

   ```http
   GET /chat HTTP/1.1
   Host: something_URL.com
   Upgrade: websocket
   Connection: Upgrade
   ...
   ```

   - 서버에서 웹 소켓 지원 여부를 반환.
   - 서버, 브라우저 모두 웹 소켓을 지원한다면 연결.

​	

​	

---

## Web Socket 상태 확인

생성된 웹 소켓 객체는 XMLHttpRequest로 생성된 객체처럼 상태를 나타내는 readyState가 존재합니다. 각각의 상태를 상수 혹은 숫자로 구분하여 사용할 수 있습니다.

| 상수       | 값   | 설명                      |
| ---------- | ---- | ------------------------- |
| CONNECTING | 0    | 연결이 수립되고 있는 상태 |
| OPEN       | 1    | 연결이 완료된 상태        |
| CLOSE      | 2    | 연결이 종료되고 있는 상태 |
| CLOSED     | 3    | 연결이 종료된 상태        |

​	

​	

---

## Web Socket 접근법

각 프로퍼티와 상태 상수에 접근하는 방법

```javascript
const socket = new WebSocket("wss://something_URL.com");

console.log(socket.readyState, Websocket.CONNECTING);
```

웹 소켓이 정상적으로 생성되면 4개의 이벤트를 사용할 수 있습니다. 각각의 이벤트는 on(eventName)혹은 addEventListener()를 통해 등록할 수 있습니다. 

- open: readyState가 OPEN 되었을 때, 즉 데이터를 주고 받을 준비가 되었을 때 발생

- close: readyState가 CLOSED가 되었을 때, 즉, 연결이 종료되었을 때 발생

- message: 서버로부터 메시지를 받았을 때 발생. 이때 이벤트 객체를 통해 수신된 메세지에 접근할 수 있음.

- error: 에러가 발생했을 때 발생

  ```javascript
  const socket = new WebSocket("wss://something_URL.com");
  
  socket.addEventListener('open', () => {
      console.log('연결 완료')
  })
  
  socket.addEventListener('message', (event) => {
      console.log('메세지 수신', event,data)
  })
  ```

  ​	

  ​	

---

## Web Socket 현황

웹 소켓은 HTTP의 요청과 응답의 한계를 넘어선 새로운 프로토콜이지만, 실제 도입시에는 아직 고려해야할 사항들이 많다고 합니다. 구형 브라우저에 대한 지원, 주고 받는 데이터의 크기와 빈도, 에러 처리, 부하 등. 이러한 사항들을 서비스로 제공하기 충분한지 파악하고 도입하는 것을 추천합니다. 

​	



***해당 포스팅은 [기초부터 완성까지, 프런트엔드](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791165921057&orderClick=LEa&Kc=)의 내용을 공부하며 기록해 놓은 것입니다.**

​	

​		

# 👀요약

현재 진행중인 프로젝트에도 웹 소켓을 적용하여 개발중인데 추가로 [redis](https://redis.io/)와 함께 사용하고 있습니다. 이제 슬슬 AI 개발에 집중하기 위해 손을 놓고 있는데 재미있는 기술들이 계속 도입되고 있어, 이래저리 정신없는 상태에서 정리된 글이라 더 깊이 있는 내용은 기회가 되면 추가해보겠습니다.

​	

​	

​			

​	

​	

​	





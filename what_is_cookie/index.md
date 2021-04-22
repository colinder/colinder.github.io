# what is Cookie?


​	

개발을 하다보면, 유저의 로그인 기능을 고민하는 경우가 많습니다. 근데. 로그인 기능을 개발하다보면 꼭 마주치는 두 가지...

cookie와 session에 대하여 알아보겠습니다. 다만 그전에 cookie와 session이 생기게 된 배경에 대하여 같이 알아 보겠습니다.

​		

# HTTP 프로토콜의 특징

HTTP는 Connectionless(비연결성)하고, Stateless하다고 합니다.

Connectionless란, 클라이언트가 행위를 통해 서비스를 제공받기 위해 서버에 request(요청)를 하면 서버는 클라이언트의 요청값에 따라 클라이언트에게 response(응답)하게 됩니다. 이렇게 한번의 request — response의 결과로 클라이언트는 어플리케이션이 제공하는 서비스를 받게되면서, 서로의 접속을 끊게 된다는 특성입니다.

Stateless란, 접속을 끊는 순간 서버와 클라이언트간의 통신이 끊키고 상태정보를 유지하지 않는다는 특성입니다.

이 두가지 특성은 장점이자 단점이 되는데, 접속을 유지함에 따르는 리소스를 줄일 수 있는 장점과 통신을 할때마다 클라이언트 인증을 해야한다는 단점이 있습니다. 그리고 이러한 단점을 보완하기 위해 쿠키, 세션이 생기게 되었습니다.

​	

# 쿠키(cookie)란?

웹 서버가 브라우저에게 지시하여 사용자의 로컬 컴퓨터에 파일 또는 메모리에 **사용자 식별 정보를 저장하는 작은 테이터 파일**입니다. 어떤 말인지 모르겠죠? ㅎ..

📃 쿠키는 주로 아래의 세 가지 목적을 위해 사용됩니다.

1. **세션 관리**(Session Management) 로그인, 사용자 닉네임, 접속 시간, 장바구니 등의 서버가 알아야할 정보들을 저장합니다.

2. **개인화**(Personalization) 사용자마다 다르게 그 사람에 적절한 페이지를 보여줄 수 있습니다.

3. **트래킹**(Tracking) 사용자의 행동과 패턴을 분석하고 기록합니다.

   ​	

흐름을 보며 더 구체적으로 알아봅시다!

<image src="/images/cookie_00.png" width="1000px">

1. 클라이언트(사용자)가 브라우저(사용자PC)를 통해 방문한 적 없는 페이지에 접속하면, 서버는 “세션 식별자(session identifier)” 정보를 담아 쿠키를 설정하여 제공(응답)한다.

2. **제공받은 cookie는 클라이언트의 사용자PC(로컬 하드)에 저장된다.**

   `A FEW TIMES LATER....`

3. 나중에 클라이언트가 재접속시 사용자PC(로컬 하드)에 있던 Set-cookie값을 서버측으로 전송한다.

4. 서버측에서는 쿠키로 사용자를 인식하고 서비스 로직에 따라 클라이언트의 cookie값을 update 하게 된다.

_*새로운 페이지에 접근하기전에는 클라이언트에게 쿠키가 없습니다._

_*서버는 쿠키를 보낼때 이 쿠키를 사용할 Domain과 Path정보까지 함께 브라우저로 전송합니다. 즉, Domain과 Path는 쿠키를 어느 도메인의 어느 패스에서 인증하는데 사용할지 브라우저에게 알려주고 브라우저는 Domain과 Path가 일치할 때만 쿠키를 서버로 전송해서 사용자를 인증 받습니다. 일단 그렇게 알고 봅시다ㅎㅎ._

​		

근데 쿠키에는 “세션 식별자(session identifier) 정보”가 담겨 있다고 했는데 쿠키는 어떻게 구성되어 있을까요? 또 어떻게 접근해서 정보를 보는 걸까요? 알아봅시다!

## 쿠키 문법

```javascript
// 쿠키는 "이름=값" 페어로 시작됩니다.
Set-Cookie: <cookie-name>=<cookie-value> 
Set-Cookie: <cookie-name>=<cookie-value>; Expires=<date>
Set-Cookie: <cookie-name>=<cookie-value>; Max-Age=<non-zero-digit>
Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>
Set-Cookie: <cookie-name>=<cookie-value>; Path=<path-value>
Set-Cookie: <cookie-name>=<cookie-value>; Secure
Set-Cookie: <cookie-name>=<cookie-value>; HttpOnly

Set-Cookie: <cookie-name>=<cookie-value>; SameSite=Strict
Set-Cookie: <cookie-name>=<cookie-value>; SameSite=Lax

// Multiple directives are also possible, for example:
Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>; Secure; HttpOnly
```

​		

## 쿠키 읽기

`document.cookie` 프로퍼티를 이용하면 브라우저에서도 쿠키를 확인할 수 있습니다.

```javascript
// ex) 
alert( document.cookie ); // cookie1=value1; cookie2=value2;...	 // 모든 쿠키를 알림창으로 확인하겠다.는 명령어

// 어디든 사이트에 들어가서 개발자 도구창을 열고 console.log(document.cookie) 를 입력해 보세요!
```

`document.cookie`는 `name=value`쌍으로 구성되어있고, 각 쌍은 `;`로 구분합니다. *(ex. document.cookie = "cookiename=value; path=/; expires=0; domain=.tistory.com")* 이때, 쌍 하나는 하나의 독립된 쿠키를 나타냅니다.

정규 표현식이나 배열 관련 함수를 함께 사용해 `;`을 기준으로 `document.cookie`의 값을 분리하면 원하는 쿠키를 찾을 수 있습니다.

​	

## 쿠키 구성요소

쿠키는 다음의 요소들로 구성됩니다.

1. name (쿠키의 이름) `필수`
2. value (쿠키에 저장된 값) `필수`

3. domain (쿠키를 발급해준 서버의 도메인(사이트) 주소)

   - `domain=site.com`

     쿠키에 접근 가능한 domain(도메인)을 지정합니다. 다만, 몇 가지 제약이 있어서 아무 도메인이나 지정할 수 없습니다.

     `domain` 옵션에 아무 값도 넣지 않았다면, 쿠키를 발급받은 사이트(도메인)에서만 쿠키에 접근할 수 있습니다. **`site.com`에서 생성한 쿠키를 `other.com`에선 절대 전송받을 수 없습니다.**

     이 외에 까다로운 제약사항이 하나 더 있습니다. **서브 도메인(subdomain)** (ex. `forum.site.com`)**에서도 쿠키 정보를 얻을 수 없다는 점입니다.**

     ```javascript
     // ex) site.com에서 user가 colinder라는 쿠키를 설정함
     document.cookie = "user=colinder"
     
     // site.com의 서브도메인인 forum.site.com에서 쿠키에 접근하려 함
     alert(document.cookie); // 찾을 수 없음
     ```

     이런 제약사항은 안정성을 높이기 위해 만들어졌습니다. 민감한 데이터가 저장된 쿠키는 관련 페이지에서만 볼 수 있도록 하기 위함입니다.

     하지만, `forum.site.com`과 같은 서브 도메인에서 `site.com`에서 생성한 쿠키 정보를 얻을 방법이 있습니다. `site.com`에서 쿠키를 설정할 때 `domain` 옵션에 루트 도메인인 `domain=site.com`을 명시적으로 설정해 주면 됩니다.

     

     ```javascript
     // site.com에서
     // 서브 도메인(*.site.com) 어디서든 쿠키에 접속하게 설정할 수 있습니다.
     document.cookie = "user=colinder; domain=site.com"
     
     // 이렇게 설정하면
     // forum.site.com와 같은 서브도메인에서도 쿠키 정보를 얻을 수 있습니다.
     
     alert(document.cookie); // user=colinder 쿠키를 확인할 수 있습니다.
     ```

     하위 호환성 유지를 위해 (`site.com` 앞에 점을 붙인) `domain=.site.com`도 `domain=site.com`과 동일하게 작동합니다. 오래된 표기법이긴 하지만 구식 브라우저를 지원하려면 이 표기법을 사용하는 것이 좋습니다.
     
     이렇게 `domain` 옵션값을 적절히 사용하면 서브 도메인에서도 쿠키에 접근할 수 있습니다.

4. path (쿠키로 사용자를 인증받을 수 있는 경로 설정)

   - `path=/`

     [서버 이름 뒤에 오는 경로](https://opentutorials.org/course/3387/21745)(ex. site.com**/login**, site.com**/main** 등)에 따라 쿠키 사용여부가 결정됩니다. 위와 같이 슬래쉬( / )로 설정하면 모든 path에서 쿠키를 사용할 수 있습니다. (미 지정시) 기본값은 현재 경로입니다.

     `path=/admin` 옵션을 사용하여 설정한 쿠키는 `/admin`과 `/admin/something`에선 볼 수 있지만, `/home` 이나 `/adminpage`에선 볼 수 없습니다.

     특별한 경우가 아니라면, `path` 옵션을 `path=/`같이 루트로 설정해 웹사이트의 모든 페이지에서 쿠키에 접근할 수 있도록 합니다.

5. expires (쿠키의 만료시간)
   - 쿠키가 언제 삭제되는지 결정합니다.

   - 쿠키는 `지속 쿠키(Persistent Cookie)`와 `세션 쿠키(Session Cookie)`로 나눌 수 있습니다.

     - `지속 쿠키(Persistent Cookie)`
       - 만료 날짜/시간(`expires` 나 `max-age` )을 지정하지 않은 쿠키
       - **파일로 저장**되므로 __브라우저가 종료되어도 쿠키는 남아있습니다.__
     - `세션 쿠키(Session Cookie)`
       * 만료 날짜/시간(`expires` 나 `max-age` )을 지정한 쿠키
       * **브라우저 메모리에 저장**되므로 **브라우저가 종료되면 쿠키는 사라지게 됩니다.**

     - `expires`와 `max-age`

       - **`expires`**

         ex) expires=Tue, 19 Jan 2038 03:14:07 GMT

         브라우저는 설정된 유효 일자까지 쿠키를 유지하다가, 해당 일자가 도달하면 쿠키를 자동으로 삭제합니다. (옵션값을 과거로 지정하면 쿠키는 삭제됩니다.)

         *쿠키의 유효 일자는 반드시 GMT(Greenwich Mean Time) 포맷으로 설정해야 합니다. `date.toUTCString`을 사용하면 해당 포맷으로 쉽게 변경할 수 있습니다. 아래는 유효 기간이 하루인 쿠키를 만드는 예시입니다.*

         ```javascript
          // 지금으로부터 하루 후
          let date = new Date(Date.now() + 86400e3);
          date = date.toUTCString();
          document.cookie = "user=colinder; expires=" + date;
         ```

       - **`max-age`**

         ex) max-age=3600

         `max-age`는 `expires` 옵션의 대안으로, 쿠키 만료 기간을 설정할 수 있게 해줍니다. 현재부터 설정하고자 하는 만료일시까지의 시간을 **초**로 환산한 값을 설정합니다. (0이나 음수값을 설정하면 쿠키는 바로 삭제됩니다.)

         ```javascript
         // 1시간 뒤에 쿠키가 삭제됩니다.
         document.cookie = "user=colinder; max-age=3600";
         
         // 만료 기간을 0으로 지정하여 쿠키를 바로 삭제함
         document.cookie = "user=colinder; max-age=0";
         ```

         *다른 브라우저들은 둘 다(`Expires` 와 `Max-Age)` 지정되었을 때 `Max-Age` 값을 더 우선시합니다.*

6. secure (쿠키 보안 설정)

   - 이 옵션을 설정하면 HTTPS로 통신하는 경우에만 쿠키가 전송됩니다.

     `secure` 옵션이 설정된 경우, `http`**`s`**`://site.com`에서 설정한 쿠키는 `http://site.com`에서 접근할 수 없습니다. 쿠키에 민감한 내용이 저장되어 있어 암호화되지 않은 HTTP 연결을 통해 전달되는 걸 원치 않는다면 이 옵션을 사용하면 됩니다.

     **`secure` 옵션을 설정하지 않으면 **`http://site.com`**에서 설정(생성)한 쿠키를 **`http`**`s`**`://site.com`**에서 읽을 수 있고,** `http`**`s`**`://site.com`**에서 설정(생성)한 쿠키도 **`http://site.com`**에서 읽을 수 있습니다.**

     쿠키는 기본적으로 도메인만 확인하지 프로토콜을 따지진 않기 때문입니다.

     ```javascript
     // (https:// 로 통신하고 있다고 가정 중)
     // 설정한 쿠키는 HTTPS 통신시에만 접근할 수 있음
     document.cookie = "user=John; secure";
     ```

7. samesite (쿠키 보안 설정)

   - 또 다른 보안 속성인 `samesite` 옵션은 [크로스 사이트 요청 위조(cross-site request forgery, XSRF)공격](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0)을 막기 위해 만들어진 옵션입니다. 이 옵션엔 두 가지 값을 설정할 수 있습니다.

     - **`samesite=strict`(값을 설정하지 않고 그냥 `samesite` 옵션만 써줘도 동일하게 동작함)**

       사용자가 사이트 외부에서 요청을 보낼 때, `samesite=strict` 옵션이 있는 쿠키는 절대로 전송되지 않습니다.

       메일에 있는 링크를 따라 접속하거나 `evil.com`과 같은 사이트에서 폼을 전송하는 경우 등과 같이 제3의 도메인에서 요청이 이뤄질 땐 쿠키가 전송되지 않죠.

       인증 쿠키에 `samesite` 옵션이 있는 경우, XSRF 공격은 절대로 성공하지 못합니다. `evil.com`에서 전송하는 요청엔 쿠키가 없을 것이고, `bank.com`은 미인식 사용자에게 지급을 허용하지 않을 것이기 때문입니다.

       이 보호장치는 꽤 믿을 만합니다. `bank.com`에서 수행하는 모든 작업은 samesite 쿠키를 함께 전송하기 때문이죠.

       하지만 약간의 불편함도 감수해야 합니다.

       만약 사용자가 메모장 등에 `bank.com`에 요청을 보낼 수 있는 링크를 기록해 놓았다가 이 링크를 클릭해 접속하면 `bank.com`이 사용자를 인식하지 못하는 상황이 발생하기 때문입니다. 실제로 이런 경우 `samesite=strict` 옵션이 설정된 쿠키는 전송되지 않습니다.

       이런 문제는 쿠키 두 개를 함께 사용해 해결할 수 있습니다. "Hello, John"과 같은 환영 메시지를 출력해주는 "일반 인증(general recognition)"용 쿠키, 데이터 교환 시 사용하는 `samesite=strict` 옵션이 있는 쿠키를 따로 둬서 말이죠. 이렇게 하면 외부 사이트를 통해 접근한 사용자도 정상적으로 환영 메시지를 볼 수 있습니다. 지급은 무조건 은행의 사이트를 통해서만 수행되도록 만들면 됩니다.

     - **`samesite=lax`**(사용자 경험을 해치지 않으면서 XSRF 공격을 막을 수 있는 느슨한 접근법)

       `strict`와 마찬가지로 `lax`도 사이트 외부에서 요청을 보낼 때 브라우저가 쿠키를 보내는 걸 막아줍니다. 하지만 예외사항이 존재합니다.

       아래 두 조건을 동시에 만족할 때는 `samesite=lax` 옵션을 설정한 쿠키가 전송됩니다.

       1. “안전한” HTTP 메서드인 경우(예: GET 방식. POST 방식은 해당하지 않음).

          안전한 HTTP 메서드 목록은 [RFC7231 명세](https://tools.ietf.org/html/rfc7231)에서 확인할 수 있습니다. 안전한 메서드는 읽기 작업만 수행하고 쓰기나 데이터 교환 작업은 수행하지 않습니다. 참고로, 링크를 따라가는 행위는 항상 GET 방식이기 때문에 안전한 메서드만 쓰입니다.

       2. 작업이 최상위 레벨 탐색에서 이루어질 때(브라우저 주소창에서 URL을 변경하는 경우).

          대다수의 작업은 이 조건을 충족합니다. 하지만 `<iframe>`안에서 탐색이 일어나는 경우는 최상위 레벨 탐색이 아니기 때문에 이 조건을 충족하지 못합니다. AJAX 요청 또한 탐색 행위가 아니므로 이 조건을 충족하지 못합니다.

       브라우저를 이용해 자주 하는 작업인 "특정 URL로 이동하기"를 실행하는 경우, `samesite=lax` 옵션이 설정되어 있으면 쿠키가 서버로 전송됩니다. 노트에 저장된 링크를 여는 것도 특정 URL로 이동하는 행위이므로 위 조건들을 충족합니다.

       하지만 외부 사이트에서 AJAX 요청을 보내거나 폼을 전송하는 등의 복잡한 작업을 시도할 때는 쿠키가 전송되지 않습니다.

       이런 제약사항이 있어도 괜찮다면, `samesite=lax` 옵션은 사용자 경험을 해치지 않으면서 보안을 강화해주는 방법으로 활용할 수 있을 것입니다.

       `samesite`는 좋은 옵션이긴 하지만, 한가지 문제점이 있습니다.

       - 오래된 브라우저(2017년 이전 버전)에선 `samesite` 옵션을 지원하지 않습니다.

       **따라서 `samesite` 옵션으로만 보안 처리를 하게 되면, 구식 브라우저에서 보안 문제가 발생할 수 있습니다.**

       구식 브라우저에 대응하지 못한다는 문제가 있긴 하지만, `samesite` 옵션을 XSRF 토큰 같은 다른 보안 기법과 함께 사용하면 보안을 강화할 수 있습니다. 구식 브라우저를 더는 사용하지 않는 때가 오면 XSRF 토큰 역시 필요하지 않겠죠.

8. HttpOnly

   - HttpOnly 속성이 붙은 쿠키는 HTTP나 HTTPS 이외의 방법으로는 사용이 안됩니다. 예컨데 javascript의 document.cookie메서드를 이용해서 쿠키정보를 획득할 수가 없습니다. 그러므로 __[cross-site scripting(XSS)](https://4rgos.tistory.com/1)__공격을 방어할 수 있다.

     ​		

     ​	

     <a hef=""><p style="text-align:right;">생각보다 분량이 많아져서 session은 다음 글로 정리해야겠습니다.</p></a>

     ​	

# 👀요약

> 쿠기 종류에 따라 인증 방식의 차이는 없으나 **데이터 저장 방식의 차이**라고 할 수 있습니다. **Persistent Cookie = 클라이언트 ,Session Cookie = 서버**
>
> 세션 쿠키의 값은 보안상 꽤나 안전한 브라우저(ex. 구글 크롬)의 메모리에 저장되기 때문에 보안에 유리하지만 파일로 저장되는 지속 쿠키의 경우 비교적으로 보안에 취약합니다.

​	

​	

## 참고 했던 글

- https://thoughtbot.com/blog/lucky-cookies
- https://www.joinc.co.kr/w/man/12/cookie
- https://ko.javascript.info/cookie#ref-201
- https://ko.javascript.info/cookie
- https://genesis8.tistory.com/220
- https://medium.com/@ddinggu/cookie%EB%9E%80-a650c6d2803e
- https://jeong-pro.tistory.com/80
- https://im-first-rate.tistory.com/33
- https://noritersand.github.io/javascript/javascript-document-cookie-%EC%BF%A0%ED%82%A4-%EC%BB%A8%ED%8A%B8%EB%A1%A4/

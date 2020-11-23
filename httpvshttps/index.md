# HTTP VS HTTPS


​	

# HTTP vs HTTPS

> 웹개발 프로젝트를 하면서 인터넷 주소창(URL)을 자주 보게 되었습니다. 그러던 중 HTTP와 HTTPS의 차이를 발견하였고 단순히 HTTPS가 보안이 더 뛰어나서 적용하면 좋다. 정도의 개념만을 가지고 있다, '한 번 차이를 깔끔히 정리하면 좋겠다.' 싶어 정리해보겠습니다.

​	

## 뜬금없이 URL란 무엇인지 우선 알아봅시다.

우리가 '인터넷 주소창'이라고 흔히 말하는 URL은 Uniform Resource Locator, 자원 위치 규약? 정도로 이해하면 좋을 것 같습니다. 

우리가 사는 주소를 잠시 생각해봅시다. 먼저, 대한민국안에 서울특별시에 있는 강남구를 생각해보면 대한민국이라는 국가에서 서울특별시라는 위치로 그리고 그 안에 강남구로 범위를 좁히며 구체적인 위치를 생각할 수 있습니다. 이렇듯. URL은 인터넷 상의 여러 페이지의 주소입니다.  예를 들어,

 https://colinder.github.io/는 https://colinder.github.io/라는 페이지를 보여주고,

 https://colinder.github.io/hugo_setting/은 https://colinder.github.io/안에 <a>hugo_setting/</a>의 페이지도 이동하는 것이죠.

URL과 웹 사이트 주소를 같은 것이라고 생각할 수 도 있는데, URL은 웹 사이트 주소보다 큰 개념이고, 웹 사이트 주소 뿐만 아니라 네트워크 상에 연결된 다양한 자원까지 포함된 개념입니다.

URL위에서 말한대로 일종의 규약인데 URL에 접속하는 경우에는 해당 URL에 맞는 *프로토콜을 알아야 합니다. *FTP은 파일 전송을 위해,  HTTP는 온라인 문서와 같은 리소스를 보기위해 사용합니다. 그리고 HTTP를 편리하게 이용할 수 있게 만든 응용 소프트웨어가 '웹 브라우저'(ex. 크롬, 사파리, 익스플로러)입니다.

​	

<p style='text-align:right; font-style:italic; color:grey'>*프로토콜: 복수의 컴퓨터 혹은 단말기와 데이터 통신을 원활하게 하기 위해 필요한 통신 규약</p>

<p style='text-align:right; font-style:italic; color:grey'>*FTP: File Transfer Protocol / 파일을 전송하는 방법 혹은 그런 프로그램</p>

​	

## HTTP (HyperText Transfer Protocol)

이제 HTTP입니다! 위에서 살짝 언급했듯이, HTTP는 인터넷에서 웹 서버와 사용자(우리) 컴퓨터에 설치된 웹 브라우저 사이에 문서([DOM](https://colinder.github.io/what_is_dom/))를 전송하기위한 통신규약입니다. 저는 HT의 뜻이, HTTP를 이해하는데 핵심이라고 생각합니다.

HyperText: 참조([하이퍼링크](https://ko.wikipedia.org/wiki/하이퍼링크))를 통해 독자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트.

간단한 클릭을 통해 여러 문서를 이동하며 내용을 볼 수 있는 것입니다.

__즉, http://colinder.github.io/ === colinder.github.io라는 주소([도메인](https://colinder.github.io/what_is_domain/))을 HyperText Transfer Protocol 형태로 받아와서 보여주세요! 정도로 해석할 수 있습니다.__

​	

그럼 HTTPS는 무엇일까요?

​	

## HTTPS (HyperText Transfer Protocol + Secure Socket)

HTTP + S(Secure Socket) 즉 HTTP에서 Secure Socket이 추가된 것이 HTTPS입니다. 간단히 HTTPS을 사용하면 __모든 Protocol이 암호화된다는 것입니다.__

​	

​	

​	









## 참고한 글

- https://post.naver.com/viewer/postView.nhn?volumeNo=16561296&memberNo=1834
- https://blog.globalhost.co.kr/19

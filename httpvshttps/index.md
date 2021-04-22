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

URL은 위에서 말한대로 일종의 규약인데 URL에 접속하는 경우에는 해당 URL에 맞는 *프로토콜을 알아야 합니다. *FTP은 파일 전송을 위해,  HTTP는 온라인 문서와 같은 리소스를 보기위해 사용합니다. 그리고 HTTP를 편리하게 이용할 수 있게 만든 응용 소프트웨어가 '웹 브라우저'(ex. 크롬, 사파리, 익스플로러)입니다.

​	

<p style='text-align:right; font-style:italic; color:grey'>*프로토콜: 복수의 컴퓨터 혹은 단말기와 데이터 통신을 원활하게 하기 위해 필요한 통신 규약</p>

<p style='text-align:right; font-style:italic; color:grey'>*FTP: File Transfer Protocol / 파일을 전송하는 방법 혹은 그런 프로그램</p>

​	

​	

## HTTP (HyperText Transfer Protocol)

이제 HTTP입니다! 위에서 살짝 언급했듯이, HTTP는 인터넷에서 웹 서버와 사용자(우리) 컴퓨터에 설치된 웹 브라우저 사이에 문서([DOM](https://colinder.github.io/what_is_dom/))를 전송하기위한 통신규약입니다. 저는 HT의 뜻이, HTTP를 이해하는데 핵심이라고 생각합니다.

- HyperText: 참조([하이퍼링크](https://ko.wikipedia.org/wiki/하이퍼링크))를 통해 독자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트.

간단히 클릭을 통해 여러 문서를 이동하며 내용을 볼 수 있는 것입니다.

__즉, http://colinder.github.io는__

__colinder.github.io라는 주소([도메인](https://colinder.github.io/what_is_domain/))을 HyperText Transfer Protocol 형태로 받아와서 보여주세요! 정도로 해석할 수 있습니다.__

---

​	

​	

그럼 HTTPS는 무엇일까요?

## HTTP<span style="color: red">S</span> (HyperText Transfer Protocol + <span style="color: red">Secure Socket</span>)

HTTP + S(Secure Socket) 즉, HTTP에서 <span style="color: red">Secure Socket</span>이 추가된 것이 HTTPS입니다. 간단히 HTTPS을 사용하면 __암호화된 Protocol을 사용한다는 뜻입니다.__

​		

### 🤔<span style="color: red">Secure Socket</span>은 무엇일까요?

> Secure Socket은 **SSL(Secure Socket Layer) 프로토콜**을 의미하는데, **컴퓨터 네트워크에 통신 보안을 제공하기 위해 설계된 암호 규약**입니다. SSL는 과거의 명칭이고, SSL이 표준화되면서 <b>TLS(Transport Layer Security)</b>로 바뀌어 사용되고 있습니다. 

보안이 어떻게 진행되는지 알아보겠습니다. 

<image src="/images//httpVShttps_00.png" width="1000px" style="display: block; margin: 10px auto;">

공개키와 대칭키로 암호문을 주고 받는 과정을 보며, 보안의 강점이 있는 것을 알았습니다.

​	

### 🙄 좋은 점은 보안뿐일까요?

HTTPS의 장점은 보안상 우위에만 있는 것이 아닙니다. **사실 HTTPS로 전환하게 되면 검색엔진 최적화(SEO)에 있어서도 큰 혜택을 볼 수 있습니다. (구글이 HTTPS 웹사이트에 가산점을 주어 검색시 노출에 혜택을 주겠다고 공표했습니다.)** 사용자들이 결국에는 가장 안전하다고 생각하는 사이트를 더 많이 방문하기 때문이기도 합니다.

<image src="/images//httpVShttps_02.png" style="display: block; margin: 10px auto;">

**또한 가속화된 모바일 페이지(AMP, Accelerated Mobile Pages)를 만들고 싶을 때도 HTTPS 프로토콜을 사용해만 합니다.** 여기서 AMP란 모바일 기기에서 훨씬 빠르게 콘텐츠를 로딩 하기 위한 방법으로 구글이 만든 것입니다. AMP는 HTML에서 불필요한 부분을 없앤 것이라고 볼 수 있습니다. 구글의 SERP(검색 결과 페이지)를 보면 스마트폰과 태블릿의 사용자들이 모바일에서 사용하기 편하도록 AMP 콘텐츠들이 두드러져 보이는 것을 볼 수 있습니다

모바일 친화적인 웹사이트를 만드는 것과 모바일 검색순위 및 지역에 SEO를 증가시키는 것이 점점 더 중요해지고 있는 요즘, HTTP를 HTTPS로 전환하는 것이 강점이 있다는 것을 알 수 있습니다.

 	

​		

​	

# 👀요약

> http와 https의 가장 큰 차이는 보안기술이 적용되었는지 아닌지가 맞았습니다. 다만, 
>
> 보안이 우수한 사이트가 노출에 이점이 있다는 점.
>
> 어떤 보안 기술이 적용되고 있는지와 어떤식으로 보안 기술이 작동하는지 알 수 있었습니다.

추가로 직접 정리한 SSL 진행 절차가 보기 어려울 수 있어서 단계별로 정리해보겠습니다. 

1. 사이트 안전성 인증 받기

<image src="/images//httpVShttps_03.png" width="1000px" style="display: block; margin: 10px auto;">

2. 사용자와 사이트 간의 인증 내용 확인

<image src="/images//httpVShttps_04.png" width="1000px" style="display: block; margin: 10px auto;">

3. 사용자와 사이트 간의 통신

<image src="/images//httpVShttps_05.png" width="1000px" style="display: block; margin: 10px auto;">



​	



## 참고한 글

- https://post.naver.com/viewer/postView.nhn?volumeNo=16561296&memberNo=1834
- https://blog.globalhost.co.kr/19
- https://12bme.tistory.com/80https://constant.kr/blog/2018/08/10/ssl-%EC%9D%B4%EB%9E%80-%EA%B5%AC%EA%B8%80%EC%97%90%EC%84%9C-ssl%EC%9D%84-%EC%A4%91%EC%9A%94%ED%95%98%EA%B2%8C-%EC%97%AC%EA%B8%B0%EB%8A%94-%EC%9D%B4%EC%9C%A0/
- http://blog.wishket.com/http-vs-https-%EC%B0%A8%EC%9D%B4-%EC%95%8C%EB%A9%B4-%EC%82%AC%EC%9D%B4%ED%8A%B8%EC%9D%98-%EB%A0%88%EB%B2%A8%EC%9D%B4-%EB%B3%B4%EC%9D%B8%EB%8B%A4/



​	

​	

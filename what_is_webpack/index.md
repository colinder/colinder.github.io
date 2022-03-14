# What is webpack?


​	

# What is webpack?

> 오늘나 자바스크립트 개발에서 모듈을 사용하여 개발하는 것은 선택이 아닌 필수입니다. 하지만 아직 모든 브라우저<span style="color:grey; font-size:0.8em">(마이크로소프트 - 앳지, 네이버 - 웨일, 구글 - 크롬 등등)</span>가 *[ES2015](https://colinder.github.io/what_is_es2015/) 모듈을 지원하지 않기 때문에 모듈 단위로 패키지를 관리할 수 없습니다. 이런 경우 전역 스코프를 공유하기 때문에 변수명이 충돌하거나 값이 덮어씌워지는 등 문제가 발생할 수 있습니다. 
>
> 이런 문제를 해결하기 위해 *번들러를 사용하는데 <b>wabpack</b>은 가장 많이 사용되는 번들러 중 하나입니다.

<image src="/images/what_is_webpack.assets/image-20220228111819340.png" width="1000px" style="margin-top:14px">

<p style='text-align:right; font-style:italic; color:grey'>*번들러<br>애플리케이션에 필요한 모든 종류의 파일들을 모듈 단위로 나누어 최소한의 파일 묶음(번들)으로 만들어 낸다. 또한 자바스크립트 파일을 외부에서 알아 보기 힘들게 코드를 변환하는 작업(Uglyfy)을 한다거나, 최신 문법의 자바스크립트를 모든 웹 브라우저에서 작동할 수 있게 ES5문법으로 변환(Transpile)하는 등 다양한 기능을 지원.</p>

​		

​		

​	

# webpack의 옵션

> webpack에는 많은 옵션들이 존재하는데 그 중 일부의 설명과 예시를 정리합니다.

webpack은 모든 것을 모듈로 다룹니다. 제 경험상 모듈은 보통 하나만 사용해선 개발이 진행되지 않고, 여러개를 동시에 사용하게 됩니다. 

<image src="/images/what_is_webpack.assets/image-20220228103930480.png" width="1000px" style="margin-top:14px">

그리고 모듈 간의 의존성에는 시작점이 존재하게 되는데 이를 **entry**로 설정합니다.

### 1. entry

```javascript
module.exports = {
    entry: "./src/js/main.js"
}
```

위의 예제 코드의 경우에, src/js 경로에 있는 main.js가 시작점(진입점)이 됩니다. 시작점이 여러개인 경우에는 옵션을 객체나 리스트로 작성할 수 있습니다.

```javascript
module.exports = {
    entry: {
        main: "./src/js/main.js",
        acticles: "./src/js/acticles.js",
    }
}

// or

module.exports = {
    entry: ["./src/js/main.js", "./src/js/acticles.js"]
}
```

​	

​	

### 2. Loader

webpack은 기본적으로 javaScript와 JSON 파일만을 이해합니다. 이외에 다양한 파일들을 이해하려면 **Loader**를 사용하면 됩니다. loader는 번들링 과정에서 이미지나 CSS파일과 같은 **자원들을 javaScript로 변환** 해줍니다.

​	

​	

### 3. Plugin

Loader가 특정 파일을 javaScript로 변환해준다면, **Plugin**은 조금 더 광범위한 역할을 수행합니다. Plugin은 **번들 파일이 생성되는 방식을 수정할 수도 있으며, 환경 변수 주입, 난도화 및 압축과 같은 작업을 수행**합니다.

​	

​	

​	

​	

***해당 포스팅은 [기초부터 완성까지, 프런트엔드](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791165921057&orderClick=LEa&Kc=)의 내용을 공부하며 기록해 놓은 것입니다.**

​		

​		

# 👀요약

이외에서 많은 webpack이 있으나 그것을 전부 알 필요는 없다고 생각합니다. 실제 개발을 하다보면 cmd에서 <span style="color:red">error : 너 이webpack 없어서 에러났어. 이거 설치하려면 "command command" 입력해</span> 라고 친절히 알려줍니다. 다만 webpack이 무엇인지? 번들이란 무엇인지? 궁금했기에 내용을 정리해봤습니다.

​	

​	

​	


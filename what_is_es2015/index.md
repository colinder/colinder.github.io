# What is ES2015?


​	

# What is ES2015?

> 웹 개발 도구와 내용을 정리하다보면 등장하던 ES2015. 그냥 어떤 느낌의 단어인지 어렴풋이 알고만 있다가 확실하게 정리해 알아두고 싶어 내용을 정리합니다.

​	

​	

​	

# 우선 javaScript에 대하여 조금 알아봅시다.

>javaScript는 프로그래밍 언어입니다. 프로그래밍 언어는 지금도 꾸준히 개선되고 발전됩니다. 계속 변화하고 있기에 특정 시기에 개발된 상태를 1.2.1과 같이 **버전**으로 등록해 배포합니다. 자바스크립트는 결점이 상당히 많은 언어였고, 사용자들이 직접 결점을 보완하는 방법으로 발전되어 왔는데, <br>**ECMA(European Computer Manufacturer's Association)**라는 단체에서 기존의 결점을 보완한 **표준 자바스크립트 버전**을 매년 발표하게 됩니다. 그리고 ES는 바로 **EcmaScript**의 줄임말입니다. 이 표준은 1997년에 처음 제정되어 계속 발전하고 있는 중입니다.

​	

​	

​	

# ECMAScript(ES)란?

ES는 자바스크립트를 이루는 코어(Core)스크립트 언어로써, 다양한 환경에서 운용될 수 있게 확장성을 갖고 있다. 확장성의 대표적인 예로 웹 브라우저에서 javaScript가 동작할 수 있도록 *BOM과 [DOM](https://colinder.github.io/what_is_dom/)을 함께 사용하는 확장성이 있다. 이러한 확장성들은 ES버전에 따른 문법과 기능의 확장을 가능하게 한다.

<p style='text-align:right; font-style:italic; color:grey'>*BOM(Browser Object Model) –  javaScript가 browser와 소통하기 위해 만들어진 모델, window 객체를 통해 접근</p>

​	

​	

# ES의 버전관리 히스토리는?

ES3 -> ES5 -> ES6(ES2015) -> ES7(ES2016)...

넘버링과 년도가 따로있다. 그래서 숫자만 보고 오해할 수 있지만, ES5는 ES2015가 아니다.

​	

​	

​	

# 👀요약

ES2015에서 엄청나게 많은 문법과 기능(클래스, 모듈, 분해대입, 템플릿 문자열, 블록 스코프, 반복자, 프록시 등등...)이 추가되고, Node.js 등 웹 브라우저 외에도 JavaScript를 구동할 수 있는 구동 환경의 종류가 많아지면서, 다른 범용 프로그래밍 언어<span style="color:grey; font-size:0.8em">(Python ...)</span>와 비교해도 전혀 뒤쳐지지 않는 범용 프로그래밍 언어가 되었습니다.

ECMAScript 관련하여 최신 변화점은 [여기](https://tc39.es/ecma262/)에서 확인할 수 있습니다.

​	

​	

​	

### 참고한 글

- https://tc39.es/ecma262/

- https://www.ecma-international.org/
- https://babeljs.io/docs/en/learn/

- https://deeds-not-words.tistory.com/entry/ES6-ES2015-ECMAScript%EB%9E%80-%EB%8F%84%EB%8C%80%EC%B2%B4-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80

- https://www.zerocho.com/category/ECMAScript/post/5756d488e9c105aaeb550ea5

​	

​	

​	

​	



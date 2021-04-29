# what is DOM?


​	

Web을 공부하다보면 반드시 듣게 되는 단어. "DOM"

근데 이게 뭔지 감이 잘 안왔고 그래서  [WIT블로그](https://wit.nts-corp.com/2019/02/14/5522)를 필사하며 공부한 내용을 추가해 정리했습니다.

​	

# DOM이란?

> DOM(Document Object Model)은 웹 페이지에 대한 인터페이스입니다. 기본적으로 페이지의 콘텐츠 및 구조, 그리고 스타일을 읽고 조작할 수 있도록 API를 제공합니다. 먼저 DOM을 이해하기 전에 웹 페이지가 어떻게 빌드 되는지 알아보면 이해하는데 도움이 됩니다. 하여 알아봅시다!

<p style='text-align:right; font-style:italic; color:grey'>*인터페이스(interface): 서로 다른 두 개의 시스템, 장치 사이에서 정보나 신호를 주고받는 경우의 접점이나 경계면이다. <br/>즉, 사용자가 기기를 쉽게 동작시키는데 도움을 주는 시스템.</p>

​	

​	

# 웹 페이지는 어떻게 만들어질까?

웹 브라우저가 원본 HTML 문서를 읽어들인 후, 스타일을 입히고 대화형 페이지로 만들어 뷰 포트에 표시하기까지의 과정을 “CRP(Critical Rendering Path)”이라고 합니다. [Understanding the Critical Rendering Path](https://bitsofco.de/understanding-the-critical-rendering-path/)을 참고해보면 6단계로 웹 페이지가 생성되는 것을 알 수 있습니다. 하지만 어렵죠. 조금 더 간추려 보자면 이 단계들은 2개로 나누어 생각해볼 수 있습니다. 

첫 번째 단계에서 **브라우저는 읽어들인 문서를 파싱하여 어떤 내용을 페이지에 렌더링할지 확인**하고 **랜더트리**를 생성합니다. 그리고 두 번째 단계에서 **브라우저는 해당 렌더링을 수행**합니다.

<image src="/images/DOM_00.jpg" width="1000px">

<p style='text-align:right; font-style:italic; color:grey'>*DOM(Document Object Model) – HTML 요소들의 구조화된 표현</p>

<p style='text-align:right; font-style:italic; color:grey'>*CSSOM(Cascading Style Sheets Object Model) – 요소들과 연관된 스타일 정보의 구조화된 표현</p>

​	

​	

설명과 그림을 보면 DOM은 웹페이지 생성에 첫번째로 수행되는 기능입니다.

그리고 _HTML 요소들의 구조화된 표현_ 이죠. 그렇다면 

# HTML은 뭘까요?

HTML(Hyper Text Markup Language)은 웹 페이지를 위한 언어로, 특정 영역이 어떤 성질을 갖는지 미리 정해진 규칙에 따라 **구조화된 요소**들로 이루어진 마크업 언어입니다.
마크업 언어는 태그(<>)를 이용하여 데이터 구조를 명명하는 언어이므로(ex. `<body>`), 태그 안에 담기는 요소들에 따라 영역의 성질이 달라지게 되고 각 영역들이 모여 구조화된 문서를 만듭니다.

또한 HTML은 인간이 이해하고 구분할 수 있는 언어로 만들어져 있고, **기계는 이렇게 규약된 언어를 해석할 수 없기** 때문에, 파싱이라는 작업을 거쳐 브라우저가 해석할 수 있는 언어와 구조로 변환하는 작업이 필요합니다.
(각 브라우저마다 파서가 다르기 때문에 같은 HTML문서라도 다른 파싱 결과값을 가질 수 있다.)

**즉, 브라우저에서 HTML을 가공?(parsing)하여 Object 객체로 다시 만들어 기계가 해석할 수 있는 모습으로 변형한 것을 DOM이라고 합니다.**

​	

​	

# DOM은 어떻게 만들어지는 걸까?

DOM은 HTML 요소들이 가공(구조화)된 표현입니다. 둘은 서로 비슷하지만, **DOM이 갖고 있는 특징은** 단순 텍스트로 구성된 HTML 문서의 내용과 구조가 객체 모델로 변환되어 **다양한 프로그램에서 사용될 수 있다는 점**입니다. 

DOM의 개체 구조는 `노드 트리`로 표현됩니다.

<image src="/images/DOM_01.jpg" width="700px">

이 HTML 구조는

<image src="/images/DOM_02.jpg" width="700px">

이와 같은 `노드 트리`의 DOM으로 구성됩니다. 

​		

​	

# 그럼 DOM과 HTML은 같은건가?

답은 '아니다.' 라고 말 할 수 있습니다. DOM은 HTML 문서로부터 생성되지만 항상 동일하지 않습니다.

​	

### 1. DOM은 HTML이 아닙니다.

DOM은 HTML 문서로부터 생성되지만 항상 동일하지 않습니다. DOM이 원본 HTML 소스와 다를 수 있는 두 가지 케이스가 있습니다.

1. 작성된 HTML 문서에 오류가 있을 때

   DOM은 유효한 HTML 문서의 인터페이스입니다. DOM을 생성하는 동안, 브라우저는 유효하지 않은 HTML 코드를 올바르게 교정합니다.

   <image src="/images/DOM_03.jpg" width="700px">

   위에 html코드에는 필수 요소인 `<head>`와 `<body>`가 빠져 있습니다. 하지만  

   <image src="/images/DOM_04.jpg" width="700px">

   DOM에는 올바르게 교정되어 있는 모습을 볼 수 있습니다. 

   ​	

 2. 자바스크립트에 의해 DOM이 수정될 때

    DOM은 HTML 문서의 내용을 볼 수 있는 인터페이스 역할을 하는 동시에 **동적 자원**이 되어 수정될 수 있습니다.
    예를 들어, 자바스크립트를 사용해 DOM에 새로운 노드를 추가할 수 있습니다.

    <image src="/images/DOM_05.jpg" width="700px">

    이 코드는 DOM을 업데이트합니다. 하지만 HTML 문서의 내용을 변경하진 않습니다.

    ​	

### 2. DOM은 브라우저에서 보이는 것이 아닙니다.

<image src="/images/DOM_06.jpg" width="1000px">

위의 그림에서 보면 DOM은 아직 CSSOM(CSS)가 적용되지 않은 상태입니다. 렌더 트리는 오직 스크린에 그려지는 것으로 구성되어 있어 DOM과 다릅니다.
달리 말하면, 렌더링 되는 요소만이 관련 있기 때문에 시각적으로 보이지 않는 요소는 제외됩니다.

예를 들어, 아래 html코드에는 p태그가 `display: none` 스타일 속성을 가지고 있는 요소입니다.

<image src="/images/DOM_07.jpg" width="700px">

렌더 트리에 해당하는 뷰 포트에 표시되는 내용은 `<p>` 요소를 포함하지 않습니다.

<image src="/images/DOM_09.jpg" width="700px" alt="랜더트리">

DOM은 `<p>` 요소를 포함시킵니다.

<image src="/images/DOM_08.jpg" width="700px" alt="DOM">

​	

### 3. DOM은 개발도구에서 보이는 것이 아닙니다.

개발도구의 요소 검사기는 DOM과 가장 가까운 근사치를 제공합니다. 그러나 **개발도구의 요소 검사기는 DOM에 없는 추가적인 정보를 포함합니다.**

가장 좋은 예는 CSS의 가상 요소입니다. `::before` 과 `::after` 선택자를 사용하여 생성된 가상 요소는 CSSOM과 렌더 트리의 일부를 구성합니다.
하지만, 기술적으로 DOM의 일부는 아닙니다. DOM은 오직 원본 HTML 문서로부터 빌드 되고, 요소에 적용되는 스타일을 포함하지 않기 때문입니다.
가상 요소가 DOM의 일부가 아님에도 불구하고, 요소 검사기에서는 아래와 같이 확인됩니다.

<image src="/images/DOM_10.jpg" width="700px" style="border: 1px solid">

이러한 이유로 가상 요소는 **DOM의 일부가 아니기 때문에 자바스크립트에 의해 수정될 수 없습니다.**

​	

***해당 포스팅은 [WIT블로그](https://wit.nts-corp.com/2019/02/14/5522)를 필사하며 공부한 내용을 기록해 놓은 것입니다.**

​	

---

## 더 공부하기

- ::before와 ::after는 `가상요소`입니다.

  https://green-webdesigner.tistory.com/20

  ​	

  ​	























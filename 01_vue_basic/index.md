# 01_Vue_basic (Vue란? & Vue 문법1)


​	

# Vue

>Vue.js는 웹 애플리케이션의 사용자 인터페이스를 만들기 위해 사용하는 오픈 소스 프로그레시브 **자바스크립트 프레임워크**
>
>간단히, `온라인 홈페이지를 만드는 프로그램`입니다. 

​	

## Vue의 이론적 구성 및 설명을 알아보자

**1. Vue는 MVVM 패턴을 따른다.**

> MVVM(Model-View-ViewModel) 패턴?
>
> 모델과 뷰 사이에 뷰모델이 위치하는 구조

{{<image src="/images/image-20200717101451510.png" caption="MVVM 패턴" width="550px">}}

​	

**2. Vue는 SPA (Single-Page Application)**

>서버로부터 완전한 새로운 페이지를 불러오지 않고 현재의 페이지를 동적으로 다시 작성함으로써 사용자와 소통하는 [웹 ](https://ko.wikipedia.org/wiki/웹_애플리케이션)[애플리케이션](https://ko.wikipedia.org/wiki/웹_애플리케이션)을 말한다.
>
>내가 이해한 방식: 변경사항이 발생했을 때 새로고침을하며 매번 페이지를 새롭게 구성하는 것이 아니라, 서버를 돌릴 때 이미 모든 페이지가 제작되어 있고 이를 사용자의 선택에 따라 보여주는 것. 

​	

## Vue의 기본 문법 및 동작 방법을 알아보자

**01_data 선언**

```vue
✨Point
1. data는 함수로 선언하여 등록하고 사용한다.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id='app'>

  </div>
  <!-- Vue를 사용하겠다는 CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app', 
      // component별로 data(변수등록)는 함수로 선언하여 사용한다.
      data: {
        message: 'Hello Vue'
      }
    })
  </script>
</body>
</html>
```

​	

**02_interpolation(보간법)**

```vue
✨Point
1. 보간법: 두 점을 연결하는 방법
2. 난, `data(변수)는 script태그에 함수 형태로 선언하며, {{}} 2개의 중괄호 안에 변수명을 적어 사용한다.` 정도로 이해했다.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id='app'>
    {{ message }}
  </div>
  <!-- CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app', 
      data: {
        message: 'Hello Vue'
      }
    })
  </script>
</body>
</html>
```

​	

**03_v-text**

```vue
✨Point
1. data에 선언된 변수를 그대로 가져와서 노출

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id='app'>
    <!-- Vanilla JS. domElenment.innText -->

    <!-- v- 로 시작하는 것들은 모두 디렉티브(명령하는 것)라고 부른다. -->
    <p v-text='message'></p>
    <p>{{ message }}</p>
    
  </div>
  <!-- CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app', 
      data: {
        message: '완전히 같아요'
      }
    })
  </script>
</body>
</html>
```

{{<image src="/images/image-20200717150036540.png" caption="실행화면" width="300px">}}



**04_v-if**

```vue
✨Point
1. 태그 안 v-if가 true일 때만, 값이 노출된다. 
2. vue에선 빈 리스트는 True값 반환
3. 이외에 false, 빈스트링(""), 0은 False값 반환

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vue-if</title>
</head>
<body>
  <div id='app'>
    <p v-if='bool1'>true는 참이니 노출</p>    <!--bool1이 참이니까. true가 보인다.-->
    <p v-if='bool2'>false</p>   <!--bool2이 거짓이니까. false가 안보인다.-->
    <p v-if='str1'>비어있지 않은 문자열은 True</p>    <!--str1이 참이니까. true가 보인다.-->
    <p v-if='str2'>비어있으면 false고 노출 안됨</p>   <!--str2이 거짓이니까. false가 안보인다.-->
    <p v-if='num1'>1은 True</p>    <!--num1이 참이니까. true가 보인다.-->
    <p v-if='num2'>0은 false</p>   <!--num2이 거짓이니까. false가 안보인다.-->
    <p v-if='arr'>빈 리스트는 arr_True!!!</p>   <!-- vue는 빈배열이 true 평가 -->
    <p v-if='arr.length'>arr_false</p>    
  </div>
  <!-- CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app', 
      data: {
        bool1:true,
        bool2:false,
        str1:'Yes',
        str2:'',
        num1:1,
        num2:0,
        arr: [],
      }
    })
  </script>
</body>
</html>
```

​	

**05_v-if-elseif-else**

```vue
✨Point
1. vue에선 if / elseif / else을 나누어 사용 가능.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id='app'>
    <!-- 조건식도 가능 -->
    <p v-if="username === master">hello master</p>
    <p v-else>hello user</p>
    <hr>
    <p v-if="number > 0">양수</p>
    <p v-else-if="number < 0">음수</p>
    <p v-else>영</p>
  </div>
    
  <!-- CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app', 
      data: {
        username: 'master',
        number: 0,
      }
    })
  </script>
</body>
</html>
```

​	

**06_v-for**

```vue
✨Point (아주아주 중요)
1. list안에 담으면 for사용 가능

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id='app'>
    <ul>
      <li v-for='number in numbers'>{{ number }}</li>
      <!-- 이런 것도 가능 -->
      <li v-for='number in numbers'>{{ number + 1 }}</li>
    </ul>
    <ol>
      <li v-for='teacher in teachers'>{{teacher}}</li>
      <li v-for='teacher in teachers'>{{teacher.name}}</li>
    </ol>
  </div>
  <!-- CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app', 
      data: {
        numbers: [0,1,2,3,4,5],
        teachers: [
          { name: 'neo'},
          { name: 'tak'},
        ]
      }
    })
  </script>
</body>
</html>
```

v-for 연관된 key라는 것이 있는데 이는 나중에 반드시 고려해야하는 상황이 오며, [공식문서](https://kr.vuejs.org/v2/guide/list.html)를 참고하면 좋다.

{{<image src="/images/image-20200717151452747.png" caption="실행화면" width="150px">}}

​	

**07_v-bind**

```vue
✨Point (아주아주 중요)
1. v-bind는 줄여서 : 로 사용할 수 있다.
2. v-bind는 `HTML 태그의 속성 값`을 그대로 사용할 수 있다.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>v-bind</title>
</head>
<body>
  <div id='app'>
    <!-- 이러면 안돼 -->
    <a href="{{ googleUrl }}">bad Google link</a>
    <br>
    <!-- v-text와 v-bind의 차이점 -->
    <a v-text:href="googleUrl">Google</a>
    <br>
    <a v-bind:href="googleUrl">Google</a>
    <br>

    <a v-bind:href="naverUrl">Naver</a>
    <!-- v-bind: 는 : 로 축약해서 쓸 수 있다. 위와 아랜 동일 -->
    <a :href="naverUrl">Naver</a>

    <hr>
    <img v-bind:src="randomImageUrl" alt="randomImg">
    <img v-bind:src="randomImageUrl" v-bind:alt="altText">
  </div>
  <!-- CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app', 
      data: {
        googleUrl: 'https://google.co.uk',
        naverUrl: 'https://naver.com',
        randomImageUrl: 'https://picsum.photos/200',
        altText: 'random-image'
      }
    })
  </script>
</body>
</html>
```

{{<image src="/images/image-20200717153650195.png" caption="실행화면" width="500px">}}



​	



​	



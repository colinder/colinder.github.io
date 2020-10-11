# AJAX(axios) 요청


​	

# AJAX

> 세상에는 다양한 web서버가 있다.
>
> 그리고 모든 web은 <strong>"요청"</strong>과 <strong>"응답"</strong>으로 통신한다. 예를 들어..
>
> > **요청**: "이미지를 보여줘"
> >
> > **응답**: "오키" or "싫어"  
>
> 그렇다면, 이 web서버들과 통신 하려면 어떻게 해야 할까?

> 대표적인 통신 방법을 AJAX라 한다. 이는 JavaScript의 라이브러리중 하나이며 **A**synchronous **J**avascript **A**nd **X**ml(비동기식 자바스크립트와 xml)의 약자며, 자체가 하나의 특정한 기술을 말하는 것이 아니며, 함께 사용하는 기술의 묶음을 지칭하는 용어이다.
>
> 브라우저가 가지고있는 XMLHttpRequest 객체를 이용해서 전체 페이지를 **새로 고치지 않고**도 페이지의 일부만을 위한 데이터를 로드하는 기법 이며 Ajax를 한마디로 정의하자면 JavaScript를 사용한 [비동기](https://colinder.github.io/10_syncasynccallback/) 통신, 클라이언트와 서버간에 XML 데이터를 주고받는 기술이라고 할 수 있겠습니다.

{{<image src="/images/Ajax.png" width="1000px">}}

​	

​	



***Ajax 중 하나인 axios에 대하여 정리한다.***

# axios

- 구형 브라우저를 지원한다.

- 응답 시간 초과를 설정하는 방법이 있다.

- JSON 데이터 자동변환이 가능하다.

- node.js에서의 사용이 가능하다.

- request aborting(요청 취소)가 가능하다.

- catch에 걸렸을 때, .then을 실행하지 않고, console창에 해당 에러 로그를 보여준다.

- return값은 Promise 객체 형태이다.

  ​	

## 🙋‍♂️사용법 (vue기준)

```bash
# Terminal에
npm install axios		👈 axios 설치
```

```bash
# main.js
import Vue from 'vue'
import App from './App.vue'
import axios from "axios"				👈 추가

Vue.config.productionTip = false
Vue.prototype.$axios = axios			👈 추가

new Vue({
  render: h => h(App),
}).$mount('#app')

# axios를 모든 components에서 사용가능하게 등록한 것 == '전역으로 설정'
```

​	

## Example

* [jsonplaceholder](https://jsonplaceholder.typicode.com/)라는 json형태의 가상테이터 요청 사이트를 이용해 테스트를 진행
* 솔직히 여기 site의 Guide만보고 따라해도 AJAX를 맛볼 수 있다. (혹시 몰라 설명하자면, Guide의 "fetch"를 "axios"로 바꾸어 사용해보면 된다. )

```vue
// 새로 프로젝트를 만들고 npm install axios 설치하고, main.js에서 전역설정해준 뒤 진행.
// test해볼 conponents인 HelloWorld.vue의 내용을 수정해서 진행.

// HelloWorld.vue

<template>
  <div>
      <h1>axios 테스트</h1>										    👈 
      <button @click="axiosTest()">axios 테스트</button>			👈
      <p>{{this.posts}}</p>										  	👈
  </div>
</template>

<script>
export default {
    name : 'HelloWorld',
    data() {														👈
      return {														👈
        posts: ''													👈
      }																👈
    },																👈
    methods: {														👈
      axiosTest() {													👈
        const baseURI = 'https://jsonplaceholder.typicode.com'		👈
        this.$axios.get(`${baseURI}/posts`)							👈
        .then((res) => {											👈
          alert('1차 요청 완료')										👈
          this.posts = res.data										👈
        })															👈
      }																👈
    },																👈
}
</script>

// 살짝 JSONPlaceholder의 공식 Guide를 따르진 않는다.
```

* [서버실행화면](https://github.com/colinder/colinder.github.io/blob/master/images/01_axios.png) => 버튼클릭 후 [결과화면](https://github.com/colinder/colinder.github.io/blob/master/images/00_axios.png) (요청이 잘 받아진 화면을 확인할 수 있다.)

  ​	

**보기 좋게 [parsing](https://www.google.com/search?q=parsing+%EB%9C%BB&rlz=1C1CHBD_koKR904KR904&oq=parsing&aqs=chrome.2.69i57j0l7.3257j0j15&sourceid=chrome&ie=UTF-8)해보자.**

```vue
// HelloWorld.vue

<template>
  <div>
      <h1>axios 테스트</h1>
      <button @click="axiosTest()">axios 테스트</button>
      <p v-for="(post, i) in posts" :key="i">{{post}}</p>			👈 수정
  </div>
</template>

<script>
export default {
    name : 'HelloWorld',
    data() {
      return {
        posts: ''
      }
    },
    methods: {
      axiosTest() {
        const baseURI = 'https://jsonplaceholder.typicode.com';
        this.$axios.get(`${baseURI}/posts`)
        .then((res) => {
          alert('1차 요청 완료')
          this.posts = res.data
        })
      }
    },
}
</script>
```

- [서버실행화면](https://github.com/colinder/colinder.github.io/blob/master/images/01_axios.png) => 버튼클릭 후 [결과화면](https://github.com/colinder/colinder.github.io/blob/master/images/02_axios.png)

  ​	

  ​	

`하지만, 여기서 의문이 생긴다. 비동기 요청인 axios를 동기적으로 처리 할 순 없을까?`

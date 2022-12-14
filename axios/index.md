# what is Axios & usage for Vue


​	

# axios 설치 및 전역변수 설정 방법

front와 back간의 통신에 자주 사용하는 axios의 설치 및 전역 변수 설정 방법을 기록합니다. 

먼저, axios의 특징은

> - 구형 브라우저를 지원한다.
> - 응답 시간 초과를 설정하는 방법이 있다.
> - JSON 데이터 자동변환이 가능하다.
> - node.js에서의 사용이 가능하다.
> - request aborting(요청 취소)가 가능하다.
> - catch에 걸렸을 때, .then을 실행하지 않고, console창에 해당 에러 로그를 보여준다.

설치부터 진행해보겠습니다. 테스트는 Vue를 활용하여 진행하였습니다.

​	

### 1. axios 설치

```bash
// Terminal에
npm install axios		👈 axios 설치
```

​	

### 2. axios 전역 설정

```vue
// main.js or main.ts

import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import vuetify from './plugins/vuetify'
import Axios from 'axios'  // 👈추가

Vue.config.productionTip = false

Vue.prototype.axios = Axios  // 👈추가

new Vue({
  router,
  store,
  vuetify,
  render: h => h(App)
}).$mount('#app')
```

두 번째 `// 👈추가`를 보면 어디서든 `axios`라는 이름으로 호출하겠다. 고 선언한 겁니다.

만약 `Vue.prototype.imAxios = Axios` 라고 선언하면 `imAxios `라는 이름으로 호출

​	

### 3. axios 사용 예시

저는 Vue 개발방법 두 가지(함수형, 클래스형) 중 클래스 형으로 개발하고 'vue-property-decorator'라는 라이브러리를 적용하여 개발하였습니다.

하여 구조가 다른 개발자분들과 다를 수 있습니다.

```vue
<template>
  <div>
    <h1>axios Test</h1>
  </div>  
</template>

<script>
import {Component, Vue} from 'vue-property-decorator';

@Component({
  name: 'Test',
  components: {
  }
})
export default class Test extends Vue {
  mounted() {
    this.axios.post('URL', prams)
      .then((res) => {
        console.log(res)
      })
      .catch((err) => {
        console.error(err)
      })      
  }  
}
</script>

<style>

</style>
```

간단히 하자면, Vue의 특징인 SPA방식으로 해당 코드의 화면이 mounted 될 때,

`'URL'` 주소로 `prams`를 담아 `POST 요청`을 보낸다. 

요청에 성공하면(`.then() 동작`) console.log로 응답(res)온 사항을 출력한다.

요청을 실패하면(`.catch() 동작`) console.error로 응답(err)온 사항을 출력한다.

​	

​	

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
      <h1>axios 테스트</h1>										👈 
      <button @click="axiosTest()">axios 테스트</button>			👈
      <p>{{this.posts}}</p>									  👈
    </div>
  </template>
  
  <script>
  import {Component, Vue} from 'vue-property-decorator';
  
  @Component({
    name: 'HelloWorld',
    components: {
    }
  })
  export default class HelloWorld extends Vue {
    posts: ''
    
    axiosTest() {
      const baseURI = 'https://jsonplaceholder.typicode.com'
      this.axios.get(`${baseURI}/posts`)
        .then((res) => {
          alert('1차 요청 완료')										
          this.posts = res.data 
        })
    }													
  }
  </script>
  
  // 살짝 JSONPlaceholder의 공식 Guide를 따르진 않는다.
  ```

* [서버실행화면](https://github.com/colinder/colinder.github.io/blob/master/images/axios_01.png) => 버튼클릭 후 [결과화면](https://github.com/colinder/colinder.github.io/blob/master/images/axios_00.png) (요청이 잘 받아진 화면을 확인할 수 있다.)

  ​	

- **보기 좋게 [parsing](https://www.google.com/search?q=parsing+%EB%9C%BB&rlz=1C1CHBD_koKR904KR904&oq=parsing&aqs=chrome.2.69i57j0l7.3257j0j15&sourceid=chrome&ie=UTF-8)해보자.**

  ```vue
  // HelloWorld.vue
  
  <template>
    <div>
      <h1>axios 테스트</h1>
      <button @click="axiosTest()">axios 테스트</button>
      <p v-for="(post, i) in posts" :key="i">{{post}}</p>			👈 수정
    </div>
  </template>
  
  <script> // 변화 없음
  export default class HelloWorld extends Vue {
    posts: ''
    
    axiosTest() {
      const baseURI = 'https://jsonplaceholder.typicode.com'
      this.axios.get(`${baseURI}/posts`)
        .then((res) => {
          alert('1차 요청 완료')										
          this.posts = res.data 
        })
    }													
  }
  </script>
  ```

- [서버실행화면](https://github.com/colinder/colinder.github.io/blob/master/images/axios_01.png) => 버튼클릭 후 [결과화면](https://github.com/colinder/colinder.github.io/blob/master/images/axios_02.png)

  ​	

  ​	

`하지만, 여기서 의문이 생긴다. 비동기 요청인 axios를 동기적으로 처리 할 순 없을까?`


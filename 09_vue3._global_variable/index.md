# 09_Vue3._global_variable


​	

# A singularity in vue3.

> vue2. 버전의 프로젝트 경험만 있었기에 최근에 새롭게 웹 개발 프로젝트를 준비하면서 변화된 Vue3. 버전에 대한 몇 가지 내용들을 정리합니다.

​			

## Vue3. 전역 변수(Global Variable) 등록

> 간단하게 axios를 사용해보려고 했는데 Vue2. 버전과 Vue3. 버전의 main.js 초기 세팅이  달랐습니다. 
> 한참을 헤매다 방법을 찾아 기록으로 남김니다.  

​		

#### 1. 프로젝트 생성 후 axios 설치

```bash
# 프로젝트 폴더(package.json이 있는 폴더)에서

npm install axios
```

​	

#### 2. 전역 설정을 위한 main.js 수정

```javascript
// main.js 초기 모습

import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

createApp(App).use(router).mount('#app')
```

```javascript
// main.js 변경

import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import axios from 'axios'		//👈

const app = createApp(App)		//👈
app.use(router)					//👈
app.mount('#app')				//👈

app.config.globalProperties.$axios = axios  //👈

// 위에서 $axios가 axios를 담은 변수 명.
```

​	

#### 3. 사용하기

```javascript
// 사용하려는 .vue 파일에서 

methods: {
    axiosTest: function() {
      this.$axios.get('https://jsonplaceholder.typicode.com/users/2')  //👈 this.변수 형태로 사용
        .then(res => console.log(res))
        .catch(error => console.error(error))
    }
}
```



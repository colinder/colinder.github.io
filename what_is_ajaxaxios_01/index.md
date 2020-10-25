# what is AJAX(axios)?_01


​	

`비동기 요청인 axios를 동기적으로 처리 할 순 없을까?`

​	

# 일단 테스트 해보자.

​	

### 1. 첫 번째 방법(.then()안에 순서대로 코딩)

```vue
<template>
  <div>
      <h1>axios 테스트</h1>
      <button @click="axiosTest()">axios 테스트</button>
      <p v-for="(post, i) in posts" :key="i">{{post}}</p>
  </div>
</template>

<script>
export default {
    name : 'Accounts',
    data() {
      return {
        posts: 'data의 posts입니당.'
      }
    },
    methods: {
      axiosTest() {
        const baseURI = 'https://jsonplaceholder.typicode.com';
        this.$axios.get(`${baseURI}/posts`)
        .then((res) => {
          alert('1차 요청 접수')			//👈 1번째 요청(시작 alert 띄우고)
          this.posts = res.data			//👈 2번째 요청(axios로 데이터를 받아오고)
          alert('1차 요청 종료')			//👈 3번째 요청(새로운 alert을 띄어보자.)
        })
      }
    },
}
</script>
```

*실제 이벤트 진행: [첫 번째]((https://github.com/colinder/colinder.github.io/blob/master/images/axios_03.png) => [두 번째]((https://github.com/colinder/colinder.github.io/blob/master/images/axios_04.png) => [세 번째]((https://github.com/colinder/colinder.github.io/blob/master/images/axios_05.png)*

이렇게는 안된다. 

​	

### 2. 두 번째 방법(.then()을 여러개 사용)

```vue
<template>
  <div>
      <h1>axios 테스트</h1>
      <button @click="axiosTest()">axios 테스트</button>
      <p v-for="(post, i) in posts" :key="i">{{post}}</p>
  </div>
</template>

<script>
export default {
    name : 'Accounts',
    data() {
      return {
        posts: 'data의 posts입니당.'
      }
    },
    methods: {
      axiosTest() {
        const baseURI = 'https://jsonplaceholder.typicode.com';
        this.$axios.get(`${baseURI}/posts`)
        .then((res) => {
          alert('1차 요청 접수')				//👈 1번째 요청(시작 alert 띄우고)
          this.posts = res.data				//👈 2번째 요청(axios로 데이터를 받아오고)
        })
        .then(()=>{
          alert('2차 요청 접수')				//👈 3번째 요청(시작 alert 띄우고)
          this.posts = "2차 요청 반영확인"				//👈 4번째 요청(시작 alert 띄우고)
        })
      }
    },
}
</script>
```

*실제 이벤트 진행: [첫 번째]((https://github.com/colinder/colinder.github.io/blob/master/images/axios_03.png) => [두 번째]((https://github.com/colinder/colinder.github.io/blob/master/images/axios_06.png) => [세 번째]((https://github.com/colinder/colinder.github.io/blob/master/images/axios_07.png)*

위의 코드에서 `this.posts = res.data  //👈 2번째 요청(axios로 데이터를 받아오고)` 이 부분이 확인되지 않습니다.

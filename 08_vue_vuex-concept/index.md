# 08_Vue_Vuex Concept...


​		

# Vuex 핵심컨셉(getters, mutations, actions, modules)

---

​	

## 🤔여기까지 왔다면, 이 흐름을 보자.

<image src="/images/vuex.png" caption="흐름을 보자" width="100%">

이제 다시 컨셉들을 보자

​	

​	

## 3. Actions 

>[공식문서](https://vuex.vuejs.org/kr/guide/actions.html)에 따르면 `액션(Actions)`은 `변이(mutations)`와 유사하나, 몇가지 다른 점이 있다고 한다.
>
>1. 상태를 변이시키는 대신 액션으로 변이에 대한 커밋을 합니다.
>2. 작업에는 임의의 비동기 작업이 포함될 수 있습니다.

---

__Q.__ *상태를 변이시키는 대신 액션으로 변이에 대한 커밋을 합니다.*

__A.__ mutations의 역할은 State를 관리하는 것이다. 그런데 만약 비동기적 요청을 마구잡이로 보내게 된다면, State가 변질될 가능성이 높아진다. 결국 비동기 요청의 경우에도 mutations에 정의한 메소드의 형태로 보내어 상태가 변화하는 것을 추적한다. 

​	

__Q.__ *작업에는 임의의 비동기 작업이 포함될 수 있습니다.*

__A.__ 예를 들어 axios요청을 보내놓고 store에 등록된 자료를 가져오거나 수정해 사용하고 싶은 경우 사용하겠다. 

​	-	이론적인 개념을 조금 더 생각해보자면, mutations의 경우 디버깅을 위해 동기(순차)적으로 작동이 되었으나 actions의 경우 비동기(비순차)적인 역할을 수행하는데 도움이 된다. 예를 들면, axios요청을 보낸다 던가. setTimeout()을 설정해 작동 시간을 조정한다던가.

<image src="/images/VuexActions.png" caption="이런 흐름으로 작동한다." width="700px">

​	

🙋‍♂️간단히 Actions을 실습해 봅시다. (Step. 1, 2로 구성)

**Step. 1**

```vue
// store.js

import Vue from "vue"
import Vuex from "vuex"

Vue.use(Vuex)

export default new Vuex.Store({
	state: {
    first: '첫번째 데이터입니다.',
    second: 2,
    listData: [
      {name: "john"},
      {name: "poul"},
      {name: "kim"}
    ]
  },
  getters: {
    addOne(state) {
      return state.second + 1
    }
  },
  mutations: {                         
    addOneMutations(state) {           
      return state.second++            
    }                                  
  },
  actions: {                                     👈
    //비동기 요청인 setTimeout을 실습             👈
    delayFewMinutes(context) {                   👈 context는 그냥 선언적인 겁니다.
      return setTimeout(() => {                  👈 
        context.commit('addOneMutations');       👈 actions도 결국 commit 으로 
      }, 1000)                                   👈 mutations를 불러오는 겁니다.
    }                                            👈 'addOneMutations'는 위에 mutations에
  }                                              👈 선언한 것을 불러온 것이고 / 1000은 1초를 의미
})
```

​	

**Step. 2**

```vue
//사용하는 component에서 

<template>
  <div>
    <h1>state그냥 불러온 값: {{change1}}</h1>
    <br>
    <h1>Getters 사용한 값: {{useGetters}}</h1>
    <br>
    <h1>Mutations 사용한 값: {{$store.state.second}}</h1>
    <button @click="useMutations">+</button>
    <hr>
    <h1>Actions 사용한 값: {{$store.state.second}}</h1>    👈
    <button @click="useActions">+</button>                👈
    <hr>                                                  👈
  </div>
</template>

<script>
export default {
  name: 'B',
  computed: {
    change1() {
      return this.$store.state.second
    },
    useGetters() {
      return this.$store.getters.addOne
    }
  },
  methods: {
    //mutations을 이용할 떄는 commit을 사용
    useMutations() {
      return this.$store.commit("addOneMutations", {N: 2})
    },
    //actions를 이용할 떄는 dispatch를 사용                 👈
    useActions() {                                         👈
      return this.$store.dispatch("delayFewMinutes")       👈 actions은 store에서 
    }                                                      👈 'dispatch'로 불러옵니다.
  }
}
</script>

<style>

</style>
```

<image src="/images/VuexActions_01.png" caption="Actions 사용한 값: 2 아래 + 버튼을 누르면 1초 뒤에 state가 변하는 것을 알 수 있다." width="700px">

​	

**✋잠깐.** 

Actions도 인자를 넘길 수 있는데!

1. ```vue
   <script>
   export default {
     name: 'B',
     computed: {
       change1() {
         return this.$store.state.second
       },
       useGetters() {
         return this.$store.getters.addOne
       }
     },
     methods: {
       //mutations을 이용할 떄는 commit을 사용
       useMutations() {
         return this.$store.commit("addOneMutations", {N: 2})
       },
       //actions를 이용할 떄는 dispatch를 사용
       useActions() {
         return this.$store.dispatch("delayFewMinutes", {by: 50, time: 2000})   👈 요기요기
       }
     }
   }
   </script>
   ```

2. ```vue
   // store.js
   
   import Vue from "vue"
   import Vuex from "vuex"
   
   Vue.use(Vuex)
   
   export default new Vuex.Store({
   	state: {
       first: '첫번째 데이터입니다.',
       second: 2,
       listData: [
         {name: "john"},
         {name: "poul"},
         {name: "kim"}
       ]
     },
     getters: {
       addOne(state) {
         return state.second + 1
       }
     },
     mutations: {
       addOneMutations(state) {
         return state.second += 2
       }
     },
     actions: {
       //비동기 요청인 setTimeout을 실습
       delayFewMinutes(context, payload) {                  👈
         return setTimeout(() => {                          👈
           context.commit('addOneMutations', payload.by)    👈 이런식으로 인자로 작동시간
         }, payload.time)                                   👈 설정도 가능하고, mutations에
       }                                                    👈 payload 받는 부분 만들어서 
     }                                                      👈 거기까지도 전달 가능
   })
   ```



---

​	

## 4. Modules

>모듈은 간단하게 store에 여러개의 저장소(모듈s)를 만들어 관리한다는 것이다. [공식문서](https://vuex.vuejs.org/kr/guide/modules.html)에 따르면 _Vuex는 저장소를 **모듈** 로 나눌 수 있습니다._ 고 설명하고 있다. 

```vue
//예를 들자면 이런 모습으로.(출처: 공식문서)

const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {                         👈 여기서 모듈을 선언해주고
    a: moduleA,                      👈 각각의 모듈을 등록해주고 있는 
    b: moduleB                       👈 모습을 볼 수 있다.
  }
})
```

​	

​	

​	

​	

### ✨실습해보면서 알게된 포인트를 정리하며 마친다.

---

1️⃣ Mutations은 동기(순차)적인 경우에 __Actions은 비동기(비순차)적인 경우에 사용한다.__

- (하지만 Actions도 결국엔 Mutations를 불러와 사용한다.) 

2️⃣ __각각의 Components에서 Actions는 dispatch로 호출해 사용한다.__

- Mutations는 commit으로 불러와서 사용

3️⃣ __Modules는 store의 저장소를 분할해 등록하고 싶을 때 사용한다.__

- Vue는 단일 트리 컴포넌트 형식이니 자주 사용되진 않을 것 같다. 



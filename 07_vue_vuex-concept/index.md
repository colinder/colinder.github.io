# 07_Vue_Vuex Concept


​	

# Vuex 핵심컨셉(getters, mutations, actions, modules)

---

## 1. Getters

> 만약? A.vue와 B.vue에서 각각 store의 state에 등록된 second의 2를 `3`으로 변형해 사용하고 싶다면 어떻게 해야 할까? [저장소에 등록되어 있는 상태(state)를 변경하고 싶다면?](https://vuex.vuejs.org/kr/guide/getters.html)

<image src="/images/vuexGetters.png" caption="이런 모습으로" width="700px">

아마 귀찮겠지만, 각각의 파일에서

```vue
// A.vue

computed: {
	change() {
		return this.$store.state.second + 1
	}
}
```

```vue
// B.vue

computed: {
	change() {
		return this.$store.state.second + 1
	}
}
```

이와 같이 동일한 코드를 작성해서 사용해야 할 것이다. 

​	

__이러한 코드의 중복을 막고 state에 등록된 data를 변경해 사용하고 싶을 때 `getters`를 사용한다.__

​	

🙋‍♂️실습해보자! (Step. 1, 2로 구성)

**Step. 1**

```vue
//지난시간에 등록한 내용을 활용해서
//store.js

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
  getters: {                         👈
    addOne(state) {                  👈
      return state.second + 1        👈
    }                                👈
  }                                  👈
})
```

​	

**Step. 2**

```vue
// A.vue, B.vue 동일하게

<template>
  <div>
    <h1>state그냥 불러온 값: {{change1}}</h1>       
    <br>
    <h1>Getters 사용한 값: {{useGetters}}</h1>       👈
  </div>
</template>

<script>
export default {
  name: 'B',
  computed: {
    change1() {                              👈 그냥 store에서 
      return this.$store.state.second        👈 불러온 자료를
    },                                       👈 노출하기 위한 등록
    useGetters() {                                     👈 store에 addOne이라는  
      return this.$store.getters.addOne                👈 ✨ getters를 불러오기
    }                                                  👈 위해 작성한 코드
  }                                                    👈 입니다.
}
</script>

<style>
</style>
```

<image src="/images/vuexGetters_01.png" caption="결과 화면" width="700px">

---



​	

## 2. Mutations

> 공식문서에 [Mutations](https://vuex.vuejs.org/kr/guide/mutations.html)는 "Vuex 저장소에서 실제로 상태를 변경하는 유일한 방법은 변이하는 것"이라는 설명이 있다. 하지만 나는 Mutations는  **store.state 값을 변경하는 로직들을 의미**한다고 정리하고 싶다.



Getters 와 차이점을 알아두면 좋은데,

1. 인자를 받을 수 있다. ____ex) addOneMutations(state, 인자)

2. computed가 아닌 methods에 등록해 사용한다. 

3. computed는 **계산된**값이기에 등록하면 바로 노출이 가능하지만 **Methods**는 `함수`지 등록된 값이 아니다.

4. mutations에 등록된 로직은 작동시 **state의 값을 변경**시킨다.___(단, 새로고침시 state을 원복)

5. mutations은 `commit()`으로 호출해 사용한다.

   ​	

역시 🙋‍♂️실습해보자! (Step. 1, 2로 구성)

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
  mutations: {                         👈 
    addOneMutations(state) {           👈
      return state.second++            👈 or 2씩 증가: return state.second += 2
    }                                  👈
  }
})
```

​	

**Step. 2**

```vue
<template>
  <div>
    <h1>state그냥 불러온 값: {{change1}}</h1>
    <br>
    <h1>Getters 사용한 값: {{useGetters}}</h1>
    <br>
    <h1>Mutations 사용한 값: {{$store.state.second}}</h1>      👈
    <button @click="useMutations">+</button>                 👈  
    <hr>
    <h1></h1>
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
  methods: {                                              👈
    useMutations() {                                      👈
      return this.$store.commit("addOneMutations")        👈
    },                                                    👈
  }                                                       👈
}
</script>

<style>
</style>
```

<image src="/images/vuexMutations.png" caption="+를 한번 누른 모습" width="700px">

---

​		







### ✨실습해보면서 알게된 포인트를 정리하며 마친다.

---

1️⃣ __Getters로 가져온 값은 새로고침(F5)을 하여도 사라지지 않는다.__

- 애초에 store에서 변형한 값을 가져오니까.

2️⃣ __Mutations로 등록된 methods는 원본데이터(store.state)의 값을 변화 시킨다.__

- 단, 새로고침(F5)시 원본데이터 값이 회복된다.

3️⃣ __Mutations은 동기(순차)적으로 작동되며 commit으로 불러와 사용한다.__

- 원본데이터(state)의 값을 변경시키기 때문에 순서가 중요하다! 



​	



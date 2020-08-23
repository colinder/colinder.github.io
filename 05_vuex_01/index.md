# 05_Vuex_01


​	

# Vuex 란? [(공식문서)](https://vuex.vuejs.org/kr/)

>Vuex는 Vue.js 애플리케이션에 대한 **상태 관리 패턴 + 라이브러리** 입니다. 애플리케이션의 모든 컴포넌트에 대한 중앙 집중식 저장소 역할을 하며 예측 가능한 방식으로 상태를 변경할 수 있습니다. 또한 Vue의 공식 [devtools 확장 프로그램](https://github.com/vuejs/vue-devtools)과 통합되어 설정 시간이 필요 없는 디버깅 및 상태 스냅 샷 내보내기/가져오기와 같은 고급 기능을 제공합니다. (공식문서 설명)

​	

```vue
# 설치 명령어
npm i vuex   or   npm install vuex

# 이후 
src폴더안에 vuex폴더를 만들고 store.js라는 파일을 생성. (파일생성위치는 자유롭게 설정가능)
```

​	

그렇다면?  **`상태관리패턴`**이란 무엇인가?

​	

🤔이를 알아보기 전에 간단히 지난 시간에 알아본 props와 emit의 문제점을 생각해보자!

1. 많은 components를 통과해야 하는 prop의 경우 테이터 이동을 위한 코드가 장황해질 수 있다.

2. 형제 components는 부모를 거쳐 데이터를 이동해야 하는 불편함이 발생한다.

   {{<image src="/images/image-20200802223729072.png" caption="형제 components = 같은 가로층의 components" width="380px">}}

   심지어 손자 components 끼리 데이터를 이동하려면 엄청나게 많은 props와 emit이 작성되어야 하는데, 이런 불편함을 해소하기 위해 데이터 저장소(**STORE**)를 생성하는 것을 고민해볼 수 있다.



​	

이 때문에 **대규모 애플리케이션의 상태를 관리할 수 있는** 상태 관리 패턴이 필요해진다. 예를 들면

{{<image src="/images/image-20200802223314342.png" caption="이런 모습으로" width="380px">}}

이를 **단일 상태 트리(single state tree)** 로 데이터를 관리한다고 표현한다.



[공식문서](https://vuex.vuejs.org/kr/guide/state.html)에 이에 대한 설명이 있는데 내가 생각하는 핵심은 아래와 같다. 

> 1. 각 애플리케이션마다 하나의 (데이터)저장소만 갖게 한다.
> 2. 데이터를 저장하고 이를 불러오기만하면 되니 간편한 코딩이 가능해진다. 
> 3. **ROW 데이터**를 저장하는 곳을 지정해 관리하는 곳이 생긴다. 

이를 구현해보자.

​	

---

아까 만든 store.js에 기본적인 세팅을 진행해보자. (해당 과정은 총 3 Step으로 구성)

​	

**Step. 1**

store.js에 데이터를 등록(저장)한다. 

```vue
## store.js

import Vue from "vue"
import Vuex from "vuex"

Vue.use(Vuex)

export default new Vuex.Store({
    // 여기에 state(상태) 정보를 등록해서 사용.
	// ex) 여러형태의 데이터를 구현해보자.
	state: {
        first: '첫번째 데이터입니다.',
        second: 2,
        listData: [
            {name: "john"},
            {name: "poul"},
            {name: "kim"}
        ]
    }
})

## ✨Point: store에 state라는 곳에 데이터를 저장했다는 것을 기억하고 다음으로 넘어가자.
```

​	

**Step. 2**

저장소(**STORE**)에 데이터가 입력되었으니 이를 최상위 .js 파일(main.js)에 등록해 모든 영역에서 사용가능**(전역으로 등록)**하게 하자.

```vue
## main.js

import Vue from 'vue'
import App from './App.vue'
import router from './router'		(이전에 설명한 router도 설치되었음을 알 수 있다.)
import store from '../src/vuex/store.js'	👈(추가)

Vue.config.productionTip = false

new Vue({
  router,		(이전에 설명한 router도 설치되었음을 알 수 있다.22)
  store,	👈(추가)
  render: h => h(App)
}).$mount('#app')

```

​	

**Step. 3**

store에 저장된 데이터를 불러와 사용한다. (난 Vue_CLI를 사용해, 디폴트 파일인 About.vue에 출력한다.)

> ### ✋ 여기서 잠깐!
>
> **Q. store에 저장된 데이터를 불러오는 방법은 무엇인가요?**
>
> **A. 여러방법이 있으나 가장 Basic한 방법으로 `1. computed`를 사용해 불러오는 방법과 `2. data`를 선언해 사용하는 방법을 알아보자.**  (이 두 방법을 추천하는 이유는 LifeCycle에서 정리되어 있다.)

```vue
## About.vue  방법.1 (computed를 사용하는 방법)

<template>
  <div>
    <h1>{{result}}</h1>
  </div>
</template>

<script>
export default {
  name: 'About',
  //Step 1.
  computed: {
    result() {
      return this.$store.state
    }
  }
}
</script>
```



```vue
## About.vue  방법.2 (data 선언 후 store를 저장해 사용하는 방법)

<template>
  <div>
    <!-- Step. 3 -->
    <h1>{{result}}</h1>
  </div>
</template>

<script>
export default {
  name: 'About',
  //Step. 1
  data() {
    return {
      result: "",
    }
  },
  //Step. 2
  created() {
    this.result = this.$store.state
  }
}
</script>
```

​	

{{<image src="/images/Vuex_00.png" caption="방법에 차이가 있을 뿐 노출되는 화면은 동일하다." width="1000px">}}



---

___

{{<image src="/images/Vuex_02.png" caption="방법.1의 경우 폴더구조" width="1000px">}}

{{<image src="/images/Vuex_03.png" caption="방법.2의 경우 폴더구조" width="1000px">}}





Thanks to '이서영'💑

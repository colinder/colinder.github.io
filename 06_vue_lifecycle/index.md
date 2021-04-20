# 06_Vue_LifeCycle


​	

# Vue_LifeCycle

> 솔직히 처음 **라이프사이클**이란 것을 공부했을 때는 이게 무슨 말인지? **삶의 주기**를 왜 알아야 하는지 전혀 몰랐습니다. 이런 저에게 [Vue 공식문서](https://kr.vuejs.org/v2/guide/instance.html#%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8](https://kr.vuejs.org/v2/guide/instance.html#라이프사이클-다이어그램))에는 말했습니다. 
>
> **지금 당장 모든 것을 완전히 이해할 필요는 없지만 다이어그램은 앞으로 도움이 될 것입니다.**

*그리고, LifeCycle을 공부 하기 전 DOM에 대한 개념이 부족하다면 [what is DOM?](https://colinder.github.io/what_is_dom/)을 꼭 보고 오길 추천합니다.*

​	

Vue.js의 라이프 사이클은 크게 **Creation**, **Mounting**, **Updating**, **Destruction**으로 나눌 수 있습니다. 이는 **생성(create)되고**, **DOM에 부착(mount)되고**, **업데이트(update)되며**, **없어지는(destroy)** 4가지 과정을 말합니다.

각각의 단계에서, Vue를 사용하는 사람들을 위해 훅(Hook)을 할 수 있도록 API를 제공합니다. 일반적으로 많이 사용하는 종류로는 `beforeCreate`, `created`, `beforeMount`, `mounted`, `beforeUpdate`, `updated`, `beforeDestroy`, `destroyed`가 있습니다.

<image src="/images/Vue_LifeCycle_00.png" width="600px" style="display: block; margin: 30px auto;">

​	

## 1. Creation (컴포넌트 초기화 단계)

Creation 단계에서 실행되는 훅(hook)들이 라이프사이클 중에서 가장 처음 실행됩니다. 이 단계는 컴포넌트가 DOM에 추가되기 전이기 때문에 DOM에 접근하거나 this.$el를 사용할 수 없고, **서버 사이드 렌더링(SSR)에서는 지원되는 훅입니다.**

따라서 클라이언트 단과 서버단 렌더링, 모두에서 처리해야할 일이 있다면 이 단계에서 하면됩니다. 

*Creation단계에서 호출되는 라이프 사이클 훅은 [beforeCreate]과 [created]가 있습니다.*

- ## **beforeCreate()**

  이름처럼 가장 먼저 실행되는 **beforeCreate**훅입니다. Vue 인스턴스가 초기화 된 직후에 발생됩니다. 컴포넌트가 DOM에 추가되기도 전이어서 `this.$el`에 접근할 수 없습니다. 또한 `data`, `methods`, `watch`, `computed`, `events(vm.$on, vm.$once, vm.$off, vm.$emit)`등이 설정되기 전이라 접근할 수 없습니다.	

- ## created()

  `data`를 반응형으로 추적할 수 있게 되며 , `methods`, `watch`, `computed`, `events`등이 활성화되어 접근이 가능하게 됩니다. 하지만 아직까지 DOM에는 추가되지 않은 상태입니다. 하여 `this.$el`에는 접근할 수 없습니다. 컴포넌트 초기에 외부에서 받아온 값들로 **data를 세팅**해야 하거나 **이벤트 리스너를 선언**해야 한다면 이 단계에서 하는 것이 가장 적절합니다.

  ​		

---

## 2. Mounting (DOM 삽입 단계)

Mounting단계는 초기 렌더링 직전에 컴포넌트에 직접 접근할 수 있습니다. (서버 사이드 렌더링(SSR)시에 호출되지 않습니다.)

초기 랜더링 직전에 돔을 변경하고자 한다면 이 단계를 활용할 수 있습니다. 그러나 **컴포넌트 초기에 세팅되어야할 데이터 fetch는 created() 단계에 주로 사용하게 될 겁니다!**

*Mounting단계에서 호출되는 라이프 사이클 훅은 [beforeMount]과 [mount]가 있습니다.*

_*fetch: api를 불러오고, 정보를 내보내 주기도 하는 함수_

- ## beforeMount()

  DOM에 부착하기 직전에 호출되는 **beforeMount**훅입니다. 가상 DOM은 생성되어 있으나 실제 DOM에 부착되지는 않은 상태입니다. 하여 아직도 `this.$el`에는 접근할 수 없습니다. (서버 사이드 렌더링(SSR)시에 호출되지 않습니다.)
  
- ## mount()

  드디어! mounted 훅에서는 컴포넌트, 템플릿, 렌더링된 **`DOM에 접근할 수 있습니다.`** **`this.$el`을 비롯한 `data`, `computed`, `methods`, `watch` 등 모든 요소에 접근이 가능합니다.** 하지만 모든 하위 컴포넌트가 마운트된 상태를 보장하지는 않습니다. (서버 사이드 렌더링(SSR)시에 호출되지 않습니다.)

  mount에는 약간의 이슈가 있습니다. 위의 설명에

  "하지만 모든 하위 component가 mount된 상태를 보장하지는 않고"  **==** 여러 component가 중첩되어 사용되는 경우 부모, 자식 component의 로딩에 순서가 있기 때문입니다. 

  *순서: 부모 created => 자식 created => 자식 mounted => 부모 mounted*

  <image src="/images/Vue_LifeCycle_01.png" width="400px" style="display: block; margin: 30px auto;">

  *이때는 `this.$nextTick`을 이용한다면, 모든 화면이 렌더링 된 이후에 실행되므로 마운트 상태를 보장할 수 있습니다.

  <image src="/images/Vue_LifeCycle_02.png" width="auto" >

  ​	

---

## 3. Updating (Diff 및 재 렌더링 단계)

컴포넌트에서 사용되는 반응형 속성들이 변경되거나 어떤 이유로 재 렌더링이 발생되면 실행됩니다. 디버깅이나 프로파일링 등을 위해 **컴포넌트 재 렌더링 시점을 알고 싶을때 사용하면 되죠.** 조심스럽지만, 꽤 유용하게 활용될 수 있는 훅입니다. (서버 사이드 렌더링(SSR)시에 호출되지 않습니다.)

*Updating단계에서 호출되는 라이프 사이클 훅은 [beforeUpdate]과 [updated]가 있습니다.*

- ## beforeUpdate()

  이 훅은 컴포넌트의 데이터가 변하여 업데이트 사이클이 시작될 때 실행됩니다. 정확히는 DOM이 재 렌더링되고 패치되기 직전에 실행됩니다. 컴포넌트 초기에 `data`가 세팅되어야 한다면 `created` 훅을, 렌더링 되고 DOM을 변경해야 한다면 `mounted` 훅을 사용하면 되기 때문에, 거의 사용하지 않는 라이프 사이클 훅입니다.

- ## updated()

  이 훅은 **DOM이 재 렌더링 된 후 호출**되는 라이프 사이클 훅입니다. DOM이 업데이트 완료된 상태이므로 DOM의 종속적인 연산을 할 수 있습니다. 

  예를 들어 변경된 `data`가 DOM에도 적용된 후에 호출되는 훅입니다. (`updated`훅에서 `data`를 수정하게 되면 `update`훅이 호출 되기 때문에 무한 루프에 빠질 수 있으니 되도록이면 안쓰는 것이 좋습니다...) 또 위의 mounted와 비슷한 이유로 모든 자식 컴포넌트의 재 렌더링 상태를 보장하지는 않으며, `this.$nextTick()`을 이용해, 모든 화면이 업데이트 된 이후의 상태를 보장할 수 있습니다.

  ​	

---

## 4. Destruction (해체 단계)

컴포넌트가 제거 될 때 실행되는 라이프 사이클 훅입니다. (서버 사이드 렌더링(SSR)시에도 호출되지 않습니다.)

*Destruction단계에서 호출되는 라이프 사이클 훅은 [beforeDestroy]과 [destroyed]가 있습니다.*

- ## beforeDestroy()

  컴포넌트가 제거 되기 직전에 호출되는 라이프 사이클 훅입니다. 이 훅에서 컴포넌트는 본래의 기능들을 가지고 있는 온전한 상태이기 때문에 모든 속성에 접근이 가능합니다. 하여 이 훅에서 이벤트 리스너를 해제하거나 컴포넌트에서 동작으로 할당 받은 자원들은 해제해야 할 때 사용하기 적합한 훅입니다.

- ## destroyed()

  컴포넌트가 제거 된 후 호출되는 라이프 사이클 훅입니다. 컴포넌트의 모든 이벤트 리스너(`@click`, `@change` 등..)와 디렉티브(`v-model`, `v-show` 등..)의 바인딩이 해제 되고, 하위 컴포넌트도 모두 제거됩니다.

  ​	

  ​	

# 👀요약

> LifeCycle을 모르고 개발한다는게 **운 좋으면 되고 운 나쁘면 안되고, 하는 막무가내 개발**과 비슷하다고 느꼈습니다. 간단하게라도 흐름을 숙지하면 많은 도움이 될 것이고, 아무리 바쁘더라도 created와 mouted 정도는 알고 개발하면 확실히 도움이 됩니다.

​	

​	

---

## 참고 했던 글.

- https://vuejs.org/v2/api/#Options-Lifecycle-Hooks
- https://beomy.tistory.com/47
- https://medium.com/witinweb/vue-js-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-7780cdd97dd4
- https://wormwlrm.github.io/2018/12/29/Understanding-Vue-Lifecycle-hooks.html
- https://junsday.tistory.com/44







# 06_Vue_LifeCycle


​	

# Vue_LifeCycle

> 솔직한 고백으로, 처음 **라이프사이클**이란 것을 배웠을 때는 이게 무슨 말인지? **삶의 주기**를 왜 알아야 하는지 전혀 몰랐습니다. 이런 저에게 [Vue 공식문서](https://kr.vuejs.org/v2/guide/instance.html#%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8](https://kr.vuejs.org/v2/guide/instance.html#라이프사이클-다이어그램))에는 말했습니다. 
>
> **지금 당장 모든 것을 완전히 이해할 필요는 없지만 다이어그램은 앞으로 도움이 될 것입니다.**

그리고, LifeCycle을 공부 하기 전 DOM에 대한 개념이 부족하다면 [what is DOM?](https://colinder.github.io/what_is_dom/)을 꼭 보고 오길 추천합니다.

​	

Vue.js의 라이프 사이클은 크게 **Creation**, **Mounting**, **Updating**, **Destruction**으로 나눌 수 있습니다. 이는 **생성(create)**되고, DOM에 **부착(mount)**되고, **업데이트(update)**되며, **없어지는(destroy)** 4가지 과정을 말합니다.

각각의 단계에서, Vue를 사용하는 사람들을 위해 훅(Hook)을 할 수 있도록 API를 제공합니다. 일반적으로 많이 사용하는 종류로는 `beforeCreate`, `created`, `beforeMount`, `mounted`, `beforeUpdate`, `updated`, `beforeDestroy`, `destroyed`가 있습니다.

<image src="/images/Vue_LifeCycle_00.png" width="auto">

​	

## 1. Creation_컴포넌트 초기화 단계

Creation 단계에서 실행되는 훅(hook)들이 라이프사이클 중에서 가장 처음 실행된다. 이 단계는 component가 DOM에 추가되기 전이다. **서버 렌더링에서도 지원되는 훅이다.**

따라서 클라이언트 단과 서버단 렌더링, 모두에서 처리해야할일이 있다면 이 단계에서 하면된다. 아직 component가 DOM에 추가되기 전이기 때문에 DOM에 접근하거나 this.$el를 사용할 수 없다.

이 단계에서는 beforeCreate 훅과 Created 훅이 있다.

## **beforeCreate**

이름처럼 가장 먼저 실행되는 **beforeCreate**훅입니다. Vue 인스턴스가 초기화 된 직후에 발생됩니다. component가 DOM에 추가되기도 전이어서 `this.$el`에 접근할 수 없습니다. 또한 data, event, watch에도 접근하기 전이라 `data`, `events(vm.$on, vm.$once, vm.$off, vm.$emit)`, `methods`에도 접근할 수 없습니다.

## created

`data`를 반응형으로 추적할 수 있게 되며 `computed, methods, watch` 등이 활성화되어 접근이 가능하게 됩니다. 하지만 아직까지 DOM에는 추가되지 않은 상태입니다. 이 단계에서 data와 events가 활성화되어 접근할 수 있다. 컴포넌트 초기에 외부에서 받아온 값들로 **`data`를 세팅**해야 하거나 **이벤트 리스너를 선언**해야 한다면 이 단계에서 하는 것이 가장 적절합니다.

​		

## 2. Mounting_DOM 삽입 단계

Mounting 단계는 초기 렌더링 직전에 컴포넌트에 직접 접근할 수 있다. 서버렌더링에서는 지원하지 않는다.

초기 랜더링 직전에 돔을 변경하고자 한다면 이 단계를 활용할 수 있다. 그러나 **컴포넌트 초기에 세팅되어야할 데이터 페치는 created 단계를 사용**하는것이 낫다.

## beforeMount

DOM에 부착하기 직전에 호출되는 **beforeMount**훅입니다. 이 단계 전에서 템플릿이 있는지 없는지 확인한 후 템플릿을 렌더링 한 상태이므로, 가상 DOM이 생성되어 있으나 실제 DOM에 부착되지는 않은 상태입니다.

# Vue_basic_04 (Vuex)


​	

# Vuex 란? [(공식문서)](https://vuex.vuejs.org/kr/)

>Vuex는 Vue.js 애플리케이션에 대한 **상태 관리 패턴 + 라이브러리** 입니다. 애플리케이션의 모든 컴포넌트에 대한 중앙 집중식 저장소 역할을 하며 예측 가능한 방식으로 상태를 변경할 수 있습니다. 또한 Vue의 공식 [devtools 확장 프로그램](https://github.com/vuejs/vue-devtools)과 통합되어 설정 시간이 필요 없는 디버깅 및 상태 스냅 샷 내보내기/가져오기와 같은 고급 기능을 제공합니다. (공식문서 설명)



그렇다면?  **`상태관리패턴`**이란 무엇인가?

​	

🤔이를 알아보기 전에 간단히 지난 시간에 알아본 porps와 emit의 문제점을 생각해보자!

1. 많은 components를 통과해야 하는 prop의 경우 테이터 이동을 위한 코드가 장황해질 수 있다.

2. 형제 components는 부모를 거쳐 데이터를 이동해야 하는 불편함이 발생한다.

   {{<image src="/images/image-20200802223729072.png" caption="형제 components = 같은 가로층의 components" width="380px">}}

   심지어 손자 components 끼리 데이터를 이동하려면 엄청나게 많은 props와 emit이 작성되어야 하는데, 이런 불편함을 해소하기 위해 데이터 저장소(**STORE**) components를 생성하는 것을 고민해볼 수 있다.



​	

이 때문에 **대규모 애플리케이션의 상태를 관리할 수 있는** 상태 관리 패턴이 필요해진다. 예를 들면

{{<image src="/images/image-20200802223314342.png" caption="이런 모습으로" width="380px">}}



# Vue_DevInfo


​	

# Vue Dev_Info

> Vue 개발을 하면서 알게 된(깨닫게 된) 내용을 정리해 기록합니다. 

​			

1. Vuex 동작에 대한 고찰

   > 개인적으로 DB의 자료를 가져오는데 5개 이상의 테이블에서 DB를 가져오고, 이를 종합해 새로운 리스트 혹은 데이터를 만들어야 한다면 무조건 `vuex의 사용`을 추천합니다. 
   > 다만, vuex를 사용하는데 일반적으로 알려진 단계를 지켜야 하는 이유와 방법에 대하여 정리합니다.

   {{< mermaid >}}
   graph LR;
       A[.vue 파일] -->|".dispatch()"| B[actions]
       B --> |".commit()"|C[mutations]
   {{< /mermaid >}}

   위의 그래프를 보면 `.vue 파일`에서 `.dispatch()`를 사용에 vuex의 store.js에 등록된 `actions`의 함수를 동작하는 신호를 보냅니다.

   이어서 `actions`에서는 `.commit()`함수로 `mutations`를 동작하는 신호를 보냅니다.

   ​	

   왜 이런 단계를 만들어 냈을까요?

   vuex는 `상태`를 관리하기 위해 만들어졌습니다. `상태`는 `무결성`을 유지해야 합니다. 데이터를 보호하고, 항상 정상적인 상태를 유지해야 하는데 여러 사람이 동시에 데이터의 변화를 요청하고 처리된다면 무결성을 유지하는데 문제가 될 수 있기 때문에 동기적인 처리가 중요합니다.  

   그런데 <span style="color:green"><b>actions는 비동기</b></span>적 처리를 하며 <span style="color:blue"><b>mutatinos는 동기</b></span>적으로 처리합니다.

   이렇게 단계가 나누어져 있는 것에 이유는 1. 일단  <span style="color:green">사용자들의 요청(actions)는 접수받고(쌓아두고)</span> 2. 이를 반영하는  <span style="color:blue">처리(mutations)는 동기(순차)적으로 처리</span>하여 3. 데이터의 무결성을 유지하기 위함이라고 생각합니다. 

   간단히 선착순의 개념을 떠올려 접수는 일단 쌓아두고 내 능력에 따라 처리하는 사람과 비슷한 모양새의 일처리 방식입니다.

   공식문서를 확인해보면 [actions도 동기적으로 사용](https://vuex.vuejs.org/kr/guide/actions.html)할 수 있습니다. 다만, 굳이 필요하지 않다면, vue 개발자가 의도한 흐름대로 사용하는 것을 권장하고 싶습니다.

   

    

   

   

   

   


# Vue_DevInfo


​	

# Vue Dev_Info

> Vue 개발을 하면서 알게 된(깨닫게 된) 내용을 정리해 기록합니다. 

​			

## 1. Vuex 동작에 대한 고찰

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

공식문서를 확인해보면 [actions도 동기적으로 사용](https://vuex.vuejs.org/kr/guide/actions.html)할 수 있습니다. 다만, 굳이 필요하지 않다면, vue 개발자가 의도한 흐름대로 사용하는 것을 권장하고 싶습니다. (동기적인 actions를 실제 프로젝트에 적용해보려했으나 쉽지 않았습니다...)

​				

## 2. EventBus 사용시 주의사항

> EventBus 사용 로직을 생각해보자
>
> 어떤 동작(method)을 하여 신호나 데이터를 보내려는 파일(A)이 존재할 것이고,
> 그 신호나 데이터를 받는 파일(B)이 존재한다.
>
> 하여 보통 A에서는 어떤 동작과 함께 신호나 데이터를 보내려는 동작(`EventBus.$emit()`)을 method에 등록하고,
> B에서는 신호나 동작을 받는 것(`EventBus.$on()`)을 created나 mounted에 등록한다.

```javascript
//// 이벤트버스 생성
var EventBus = new Vue()

//// 이벤트 발행
// 신호 or 데이터를 보내려는 .vue파일 안에서
EventBus.$emit('message', 'hello world');	// message라는 이름의 신호에 'hello world'를 데이터로 보냄

//// 이벤트 수신(구독)
// 신호 or 데이터를 받으려는 .vue파일 안에서
EventBus.$on('message', function(text) {	// message라는 신호를 수신하고 전달받은 인자(text)를
    console.log(text);						// console에 찍어본다.
});

//// 이벤트 한번 만 수신
EventBus.$once('message', function(text) { 	// message라는 신호를 단 한번 만 수신한다.
    console.log(text);
});

//// 이벤트 제거
EventBus.$off('message')	// message라는 신호를 제거(보통 beforeDestroy()에 등록)
```

EventBus.$off 를 잘 사용하는 것이 중요합니다. <b>이벤트 버스는 객체가 계속 쌓이기 때문에 `off`를 해주지 않으면 첫 번째 신호에서는 1번 동작, 두 번째 신호에서는 2번 동작, 세 번째 신호에서는 3번이 동작되어 누적된 신호를 수신하게 됩니다.</b>

​				

## 3. Backend 개발의 중요성

> 이전 개발을 하면서 Front 중심의 개발을 진행하였습니다. 전임자가 DB를 신경쓰지 않아 외래키 연결 및 N : N관계 설정이 엉망이었습니다. 물론 Backend 로직으로 어느 정도 개선이 가능하지만... 
> Back은 DB 자료를 넘겨주기만 하고 Front 중심으로 개발을 해보았습니다. 
>
> 그러지 맙시다ㅎ.
>
> DB를 최대한 정교하게 설정하고 Backend에서 최대한 필요한 형태로 자료를 변환하여 Front로 넘겨주는 것이 가장 이상적인 방법이라는 생각이 들었습니다. Front에서 데이터 형태를 변환하여 사용하것도 좋지만, `.vue파일`과 `.js 파일`과 `vuex`간의 데이터 연동되는 속도에 이슈가 발생할 가능성이 매우 높습니다. lifecycle에 능통하지않고, 참조하는 파일이 많은 경우에는 최대한 back에서 처리를 한다음 front로 데이터를 넘겨줄 수 있게 기획하는 것을 권장합니다.
>
> 추가로 axios통신을 하는데 당연히 DB에 적제된 순서대로 데이터가 Front로 넘어오겠지 했는데, sort를 해주지 않으면 데이터가 무작위 순서로 전달되었습니다. sort하여 데이터를 넘겨주는 것을 권장합니다. 

​		

## 4. Vuex - actions(async await)

> 동기화 처리를 위해 async await을 사용하려 했습니다. 
> 다만 원하는 대로 개발은 되지 않았습니다. 많은 레퍼런스를 찾아보았고 방법을 찾았지만 적용하는 것은 어려웠습니다. 
>
> 원인 1.  actions에서 axios 
>
> .vue  → (.dispatch()) → actions → (.commit()) → mutations → axios 의 로직으로 개발하였습니다. 
> 하지만 제가 확인한 레퍼런스들 중 API 통신을 actions에서 axios를 사용하는 형태의 async await 구현이 많았습니다.
> 또 저는 actions에서  여러개의 .commit()을 날려 여러개의 mutations동작을 원했고 이미 개발된 형태에서 변경하는데 시간이 많이 소요될 것으로 예상해 acitions에서 API통신 하는 방식으로 개발하지 못했습니다.

#### 😂의도한 개발 흐름

{{< mermaid >}}
graph LR;
    A(.vue 파일) -->|".dispatch()"| B(actions)
    B --> |".commit()"|C[mutation 1]
    C --> |"axios"|D[backend]
    D --> |"response"|C
    C --> |"완료"|B
    B --> |".commit()"|E[mutation 2]
    E --> |"axios"|F[backend]
    F --> |"response"|E
    E --> |"완료"|B
    B --> |"next commit..."|G[mutation 3]
{{< /mermaid >}}

> 혹시 1개의 actions에서 여러개의 .commit()을 날리는데 async await을 구현하는 레퍼런스 혹은 방법을 알고있다면 조언 부탁드립니다.

​		


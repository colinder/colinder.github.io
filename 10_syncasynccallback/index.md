# 동기 & 비동기 & CallBack함수


​	

#	동기(Synchronous)란?

어떤 작업을 실행했을 때, 그 작업이 끝나고 `결과를 응답받은 뒤`에 다음 함수를 실행하는 방식.

**만약 응답값이 없다면, 무한정 기다린다.** 즉, 응답이 와야 다음 실행이 되는 방식

*ex) A실행 👉 A의 결과값 return 확인 👉 B실행*

---

​	

# 비동기(Asynchronous)란?

어떤 작업을 실행한 후 `결과값을 기다리지 않고`, 바로 다음 함수를 실행.

*ex) A실행 👉  B실행 👉 ...*

---

​	

# CallBack함수란?

비동기 처리결과로 반환되는 Callback함수

Callback함수는 특정함수에 매개변수로 전달된 함수를 의미한다. 

> *ex)*
>
> *DB에서 데이터를 가져오라는 요청 👉 데이터가 오는 중인데 👉 출력해버리면 결과값이 나오지 않는다.*
>
> But, Callback 함수로 처리결과를 받은 후에 출력을 하게 로직을 짜면 문제없이 출력된다.

​	

상황에 따라 어떤 작업이 끝났다는 것을 사용자에게 알려주거나, 

코드 내부에서 Callback함수를 받았을 경우에만 처리하는 로직등을 짜서 가시적인 개발에 도움.

​	

​	

​	

``` bash
# 개인적으로 동기는 '순차'라는 단어로 변환해 생각하면 이해하기 쉽다.
'순차'적  👉  지정된 순서대로 시작
비'순차'적  👉  순서에 상관없이 시작
```






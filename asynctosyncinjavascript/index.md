# Async To Sync in Javascript


​	

# Async to Sync in Javascript

> 누구나 웹개발을 하다보면 HTTP 통신을 통해 데이터를 가져오고 가공하고 사용하게됩니다. 그리고 동기, 비동기 처리 때문에 골치가 아파집니다. (만약 골치가 아프지 않다면.. 부럽습니다..) 더 이상 골치아프기 싫어서 정리해봅시다.👍

_*동기, 비동기 설명은 [이전 포스팅](https://colinder.github.io/sync_async_callback/)으로 갈음합니다._

​		

## Async(비동기) to Sync(동기) 방법

자바스크립트에서의 비동기를 동기로 동작시키는 대표적인 3가지의 방법을 알아보겠습니다.

1. Callback

2. Promise

3. Async / Await

​	

## 1. Callback

>개발자들 마다 정의가 조금씩 다르겠지만, 저는 **어떤 이벤트가 발생한 후, 수행될 함수로 정의해보겠습니다.**

- 예제를 보며 이해해 봅시다. 

  ```javascript
  // Callback 사용 예제
  // a와 b라는 함수를 선언
  
  function a(callback) {       // 함수 a는 callback이라는 함수를 받는다.
      const data = 'i am data' // i am data라는 값을 담은 변수 data를 생성
      callback(data)           // callback이라는 함수에 data라는 변수를 넘겨줌.
  }
  
  function b(value) {			// 함수 b는 console에 입력받은 값 value를 콘솔에 출력하는 함수.
      console.log('넘겨받은 값', value)
  }
  
  // 사용해 봅시다.
  a(function(value) {                    // 함수 a에 function을 받는데 값으로 value를 받는다. 
      console.log('넘겨받은 값', value)   // 함수 a가 실행되면 자동으로 const data를 만드는데 data에는 'i am data'라는 값이 할당. 되고 value가 된다.
  })
  
  // 위를 간소화한 코드.
  a(b)
  ```

  ​	

  위의 예제 코드가 눈에 들어왔다면, 이제 <b>비동기적 코드</b>를 만들어 봅시다.

  ```javascript
  // Callback 비동기적 코딩
  // a가 동작하고 b가 동작하길 원하며 코딩
  
  // a와 b라는 함수를 선언
  function a(callback) {       // 함수 a는 2초 후에 동작
      setTimeout(function() {
          console.log('running a....')
      }, 2000)
  }
  
  function b() {       // 함수 b는 1초 후에 동작
      setTimeout(function() {
          console.log('running b....')
      }, 1000)
  }
  
  // 실행해봅시다.
  a()
  b()
  ```

  {{<image src="/images/Async_to_Sync_in_Javascript_00.png" width="1000px">}}

  setTimeout이라는 javascript의 가장 대표적인 동작 지연 함수를 사용해 코딩해보았고 

  결과로 a → b의 순서가 아닌 함수 동작 시간에 따른 딜레이를 확인해보았습니다. 

  ​	

  어떻게 해야 내가 코딩한 순서대로 (a → b) **동기적**으로 작동하게 만들 수 있을까요?

  ```javascript
  // Callback 동기적 코딩
  // a가 동작하고 b가 동작하길 원하며 코딩
  
  // a와 b라는 함수를 선언
  function a(callback) {       // 함수 a는 2초 후에 동작
      setTimeout(function() {
          console.log('running a....')
          callback()			 // 👈 callback함수를 추가
      }, 2000)
  }
  
  function b() {       // 함수 b는 1초 후에 동작
      setTimeout(function() {
          console.log('running b....')
      }, 1000)
  }
  
  // 실행해봅시다.
  a(b)
  ```

  {{<image src="/images/Async_to_Sync_in_Javascript_01.png" width="1000px">}}

  원하는 동작 console.log('runing a....')뒤에 callback함수를 넣어 console.log('runing b....')를 동작했습니다.

  **간단해보이지만 강력하고, 막상 적용하려면 어려울 수 있습니다. 연습을 추천합니다.😊**

  근데 callback 함수는 대부분의 개발자들이 간단한 코딩에만 사용합니다. 단순히 1, 2개 정도의 요청이라면, callback 함수로 처리할 수 있지만, 만약 그 수가 많아진다면, 소위 callback HELL이 되기 때문입니다. 

  그래서 다른 방법도 알아봅시다!

  ​	

## 2. Promise

>**`Promise`** 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타냅니다. 간단히 비동기 작업에 사용되는 객체입니다.

- Promise는 <span style='background-color: black; font-weight: bold; border-radius: 10px; padding-bottom: .2rem'> <span style="color: grey">ex)</span> <span style= 'color: #569cd6'>new</span> <span style="color: #1cb50b;">Promise <span style="color: white">(</span></span><span style= 'color: #569cd6'> function <span style="color: white">(</span> <span style="color: #dcdcaa;">resolve, reject</span> <span style="color: white">)</span></span><span style="color: white;">)</span> </span>의 형태로 사용됩니다. 

  특징으로 Promise의 인자는 반드시 <span style="color: #569cd6;">function</span>을 받아야 합니다. _(동작 성공시 실행될 resolve라는 함수와 실패시 동작할 reject / 2개의 인자를 받습니다.)_ 

- Promise 는 3가지 상태가 존재합니다.

  {{< mermaid >}}
  graph LR;
      A[pending 대기상태] -->|성공| D[fulfilled 성공상태]
      A[pending 대기상태] -->|실패| E[rejected 실패상태]
  {{< /mermaid >}}



- 만약 인자를 받지 않거나 함수 이외의 값을 전달하면, Uncaught TypeError가 발생하고, 인자로 전달된 resolve 와 reject 중 하나라도 호출하지 않는다면, 영원히 pending 상태로 대기하게 됩니다.

  __또 Promise 객체들은 `.then()` 을 통해 이어질 수 있고 `.catch()` 를 통해 에러 캐치를 할 수 있습니다. `이 부분을 활용해 동기 처리를 합니다.`__

  {{< mermaid >}}
  graph LR;
      A[pending 대기상태] -->|성공| B[fulfilled 성공상태] --> |`이 흐름을 사용`| D[.then]
      A[pending 대기상태] -->|실패| C[rejected 실패상태] --> |`이 흐름을 사용`| E[.catch]
  {{< /mermaid >}}

  ```javascript
  // Promise 사용 예제
  
  function test() {                                    // test라는 동작(함수)을 만드는데
      return new Promise(function(resolve, reject) {   // Promise로 만들고 Promise는 인자로 resolve, reject를 받는다.
          try {                                        // 정상처리되면(조건은 다양하게 구성)
              resolve(                                 // resolve를 동작하는데
                  console.log('Promise resolve')       // Promise resolve를 출력하고
              )
          } catch {                                    // error가 발생하면
              reject(                                  // reject를 동작하는데
                  console.log('Promise reject')        // Promise reject를 출력한다.
              )
          }
      })
  }
  
  test()                              // test를 동작(함수실행)하는데
  .then((res) =>                      // 에러가 없다면
      console.log('정상 처리 완료')    // '정상 처리 완료'를 출력하고
  )
  .catch((err) =>                     // 에러가 발생했다면
      console.log('에러 발생')        // '에러 발생'을 출력한다.
  )
  ```

  ​				

  위의 예제 코드가 눈에 들어왔다면, 이제 <b>동기적 코드</b>를 보며 이해해봅시다.

  ```javascript
  // Promise 동기적 코딩
  // 1,2,3 순서대로 출력되길 원하며 코딩
  
  function test() {                                    // test라는 동작(함수)을 만드는데
      return new Promise(function(resolve, reject) {   // Promise로 만들고 Promise는 인자로 reselve, reject를 받는다.
          try {
              setTimeout(function() {					// AJAX등의 처리를 기다리는 '지연 시간'을 setTimeout으로 설정
                  resolve( console.log('1') )			// resolve처리를 하는데 3초의 딜레이를 주고
              },3000)
              console.log('2')                    	// resolve 
          } catch {
              reject(
                  console.log('reject running')
              )
          }
      })
  }
  function b() {
      setTimeout(function() {
          console.log('3')
      }, 1000)
  }
  
  // 실행해봅시다.
  test()
  .then(b)
  // .then(function() {b()})
  
  //////////////////////////////////////////////////////////////////////////////////////
  // .then(b) === .then(function() {b()}) === .then(() => {b()}) !== .then(b())
  // .then(b) / .then(function() {b()}) / .then(() => {b()})는 
  // "test함수에 오류가 없다면 b함수를 return 하라" 라는 의미기 때문에 동기적인 처리가 가능!
  // 반면 .then(b())는 "test함수에 오류가 없으면 b라는 함수를 실행하라" 라는 의미기 때문에
  // 순서에 상관없이 오류가 없다면 b를 즉시 실행한다.
  // 비슷하게...
  // .then(console.log('hi'))는 "요청들어오면 hi를 출력하라" 라는 의미고
  // .then(function() {console.log('hi')}) or .then(() => {console.log('hi')})는 
  // "어떤 요청의 결과로 hi를 return하라" 하는 의미기 때문에 동기적으로 동작한다.
  ```

  {{<image src="/images/Async_to_Sync_in_Javascript_02.png" width="1000px">}}

  위 설명에는 <b>1,2,3 순서대로 출력되길 원하며 코딩</b>이라고 적어놨지만, 결과 화면은 다른 것을 알 수 있습니다. 실은 <b>2,1,3 순서대로 출력되는 것을 의도</b>한 코딩입니다.

  **2**의 경우 resolve에 포함되어 있지 않기 때문에 **인터프리터 언어*  특징에 따라 진행되기 때문입니다.

  <p style='text-align:right; font-style:italic; color:grey'>*인터프리터 언어의 특징은 <a src="https://colinder.github.io/interpretervscompiler_language/">다른 포스팅</a>에 정리되어 있습니다.</p>

  ​	

  하지만 Promise 또한 단계?가 많아진다면 가독성 매우 떨어질 수 있습니다. 사실 callback과 Promise를 사용하는데 가독성에 문제가 있다면, 본인의 코드를 의심해봐야 될 수 있습니다.👀ㅎ.
  
  ​	

## 3. Async / Await

> Async(비동기) / Await(기다려!)는 비동기 처리를 하는 방법 중 하나입니다. Promise 나 Callback 보다 가독성이 좋은 코드를 짤 수 있다고 생각합니다.

- 위에 Promise를 살펴본 이유가 있습니다. Async/await의 기반이 Promise이기 때문입니다. 우리가 쓰는 모든 async 함수는 Promise를 리턴하고, 모든 await 함수는 일반적으로 Promise가 됩니다.
- Async/await는 <span style='background-color: black; font-weight: bold; border-radius: 10px; padding-bottom: .2rem'> <span style="color: grey">ex)</span> <span style= 'color: #569cd6'>async function</span> <span style="color: #dcdcaa;;">functionName<span style="color: white">(</span><span style="color: white">)</span></span><span style= 'color: #569cd6'>  <span style="color: white">{ <span style="color: #c586c0">await </span> <span style="color: #dcdcaa">promiseFunction </span></span><span style="color: white">}</span></span> </span>의 형태로 사용됩니다.

- sync function은 내부에 await 문법을 사용하기 위해 선언합니다.

- async function은 AsyncFunction 객체를 반환하며 'async function' 표현식으로만 생성됩니다.

- async function은 인자를 받을 수 있으며, 실행 후 반환 값은 Promise 입니다.

- `await 연산자는 async function 내부에서만 사용할 수 있습니다.` (외부에서 사용하면 SyntaxError가 발생.)

- await는 반환된 Promise의 resolve의 상태까지 대기하며, await의 값은 Promise의 resolve값이 반환됩니다.

- 만약 중간에 reject를 만나면 해당 async function스코프의 작업을 중단되며, 이후 스코프 작업이 진행됩니다.

  ```javascript
  // Async/Await 사용 예제
  
  function promise() {		
      return new Promise(function(resolve, reject) {
          resolve(10)
      })
  }
  
  async function foo() {
      const num = await promise()
      console.log(num)
  }
  // 돌려봅시다!
  foo()		// 10
  ```

  ​	

  위의 코드가 이해 되셨다면 <b>동기적 사용</b>에 대한 코드를 보며 이해해 봅시다.

  ```javascript
  // Async/Await 동기적 코딩
  
  function promise() {
      console.log('AsyncAwait running...')
      return new Promise(function(resolve, reject) {
          setTimeout(function() {
              resolve(10)
          }, 5000)
      })
  }
  
  async function foo() {
      console.log('foo running...')
      const num = await promise()
      console.log('num:', num)
  }
  // 돌려봅시다!
  foo()
  ```

  {{<image src="/images/Async_to_Sync_in_Javascript_03.png" width="1000px">}}

  가독성이 훨씬 뛰어나다고 생각합니다. 

  Promise를 선언하고 이를 await 걸어, 순서를 정리하는 방법으로 코딩을 하다보니 가독성은 좋지만, Promise의 사용법도 익혀야 한다는 단점이 있습니다. 

  ---

  

  ​	

# 👀요약

> 공부한 3가지 방법 모두 유용하고 강력한 기능이라는 생각입니다. 모두를 익히면 좋고 선택적으로 익혀야 한다면 본인에게 잘 맞는 방법으로 익혀 사용하면 좋을 것 같습니다.

​		

​	

# 🤷‍♂️삽질

- Async/Await 관련해서 혼자 이런 저런 테스트를 해보니 위에 설명한 것과 같이 new Promise()로 선언한 것들만 동기적으로 작동했고 일반 function으로 선언한 것들은 원하는 대로 동작하지 않았습니다. 

  또 주의할 점에

  ```javascript
  // 여러 test 중 하나. 
  
  function promise() {
      console.log('AsyncAwait running...')
      return new Promise(function(resolve, reject) {
          setTimeout(function() {
              resolve(10)
          }, 5000)
      })
  }
  
  function test2Promise() {
      console.log('test2Promise 시작')
      return new Promise(function(resolve, reject) {
          setTimeout(()=> {
              console.log('test2Promise running...')
              						// 👈 Promise의 resolve가 없습니다.
          },3000)
      }) 
  }
  
  async function foo() {
      console.log('foo running...')
      const num = await promise()
      // await test1()
      await test2Promise()
      console.log('num:', num)
  }
  // 돌려봅시다!
  foo()
  ```

  {{<image src="/images/Async_to_Sync_in_Javascript_04.png" width="1000px">}}

  test2Promise의 resolve를 등록하지 않으면 이전 Promise()인 promise()도 resolve가 정상적으로 진행되 않는 것을 볼 수 있습니다.

  위에서 설명한 

  > 만약 인자를 받지 않거나 함수 이외의 값을 전달하면, Uncaught TypeError가 발생하고, 인자로 전달된 resolve 와 reject 중 하나라도 호출하지 않는다면, 영원히 pending 상태로 대기하게 됩니다.

  가 발생한 것입니다. 

  ​	

- 공부를 하던 중 `.then()`에 대한 흥미로운 내용을 확인했습니다.

  > `.then()`은 요청의 결과가 성공했을 때 사용하는 것으로만 생각했는데 인자를 2개 받는 다는 것을 알았습니다. 확인해보니 `.then()` 에 넘겨지는 인자는 선택적(optional)이었습니다. 그리고
  >
  > `catch(failureCallback)` 는 `then(null, failureCallback)` 의 축약었습니다. 

  ​			

  ​		

## 참고 했던 글

- https://medium.com/@olaf.go/javascript-async-to-sync-157c57208598

- https://victorydntmd.tistory.com/48

- https://medium.com/@pks2974/javascript-%EC%99%80-promise-a6db8ca424ed

- https://joshua1988.github.io/web-development/javascript/js-async-await/

- https://medium.com/@pks2974/javascript-%EC%99%80-async-await-111fdad3c20d

- https://mber.tistory.com/8

- https://medium.com/@kiwanjung/%EB%B2%88%EC%97%AD-async-await-%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-%EC%A0%84%EC%97%90-promise%EB%A5%BC-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-955dbac2c4a4

- https://joshua1988.github.io/web-development/javascript/promise-for-beginners/#promise%EA%B0%80-%EB%AD%94%EA%B0%80%EC%9A%94

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise

  ​	

  ​		

**Special Thanks to kmpak.sfy✨.**

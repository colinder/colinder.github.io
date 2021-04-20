# JavaScript Hoisting


​	

# JavaScript Hoisting

>web 개발을 하면서 여러 레퍼런스들을 확인하고 공부하게 되는데, 문득 똑같아 보이나 왜 이건 되고? 왜 이건 안되지?하는 부분이 있었습니다.
>
>그 차이는 Hoisting에 대한 내용이었는데요. 알아두면 쓸모있기에 정리해봅시다.

​	

## 호이스팅(Hoisting)이란?
호이스팅을 한 줄로 설명하자면, **선언문을 <span style="color: red">유효 범위</span>의 최상단으로 끌어올리는 행위**라고 할 수 있습니다.

**최상단**이라는 표현이 중요한데요. [인터프리터 언어](https://colinder.github.io/interpretervscompiler_language/)인 자바스크립트가 한 줄씩 순서대로 코딩을 실행하는 것이 아니라, 임의로 특정 내용을 최상단으로 끌어 올려서 우선 실행하는 것을 호이스팅이라고 합니다.  

​	

### 호이스팅의 특징

- 자바스크립트 Parser가 함수 실행 전 해당 함수를 한 번 훑는다.

- 함수 안에 존재하는 변수/함수선언에 대한 정보를 기억하고 있다가 실행시킨다.

- 유효 범위: 함수 블록 {} 안에서 유효

- 실제 메모리에는 변화가 없다.

  ​	

### 호이스팅의 대상

<span style="color: red"><b>var 변수 선언</b></span>과 <span style="color: red"><b>함수선언식(Function Declarations)</b></span>에서만 호이스팅이 일어납니다.

첨언으로 <b>[ var 변수 / 함수 ]</b>의 **선언**만 호이스팅이 동작하고, <b>할당(초기화)</b>은 호이스팅이 동작하지 않습니다.

​	

#### 🙋‍♂️ var 변수 vs let 변수 비교

```javascript
// 예시 0
// 아래 코드를 전체 복사 후 붙여넣으면 결과 확인 가능
console.log(a)		// undefined
console.log(b)		// VM85:2 Uncaught ReferenceError: b is not defined
var a = "Im a"; 	// var 변수 선언 & 초기화
let b = "Im b"; 	// let 변수 선언 & 초기화

// var 변수 a는 호이스팅되어 오류가 아닌 undefined를 출력하고
// let 변수 b는 호이스팅되지 않아 error를 출력합니다.
```

만약 변수를 선언한 뒤 나중에 초기화시켜 사용한다면, 그 값은 undefined로 지정됩니다. 아래 예제로 동작 순서를 알아 봅시다.

```javascript
// 예시 1
console.log("x:", x)	     // x: undefined
var x = 1; 				 	 // x 선언 & 초기화
console.log(x + "/" + y);	 // '1/undefined'
var y = 2;				 	 // y 선언 & 초기화

// 아래 코드는 '예시 1'이 동작하는 순서를 정리한 것입니다.
var x; 						// x 선언  =>  x: undefined
var y; 						// y 선언  =>  y: undefined
console.log("x:", x)		// x: undefined 출력
var x = 1; 				 	// x 초기화  => x: 1
console.log(x + "/" + y);	// '1/undefined' 출력
var y = 2; 					// y 초기화  =>  y: 1
```

<p style='text-align:right; font-style:italic; color:grey; font-size:.9rem'><b>* [ let / const ] 변수 선언</b>과 <b>함수표현식(Function Expressions)</b>에서는 호이스팅이 발생하지 않습니다.</p>

​	

​	

#### 🙋‍♂️ 함수선언식(Function Declarations) vs 함수표현식(Function Expressions)

```javascript
// 함수 선언식(function declaration) 예시
function 함수명() {
  함수 로직 부분
}
함수명(); // 함수 사용


// 함수 표현식(function expression) 예시
var 함수명 = function () {
    return 'A function expression';
}
함수명(); // 함수 사용
```

함수선언식은 일반적인 프로그래밍 언어에서 함수 선언과 비슷한 모습니다. 다만, 함수표현식은 유연한 자바스크립트 언어의 특징을 활용한 선언 방식이라고 할 수 있습니다.

​	

​	



​	



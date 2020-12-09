# Javascript Type


​	

# Javascript Type

> Javascript Type에 대하여 알아보겠습니다.

​	

​	

## 그 전에 먼저 변수를 선언하는 방법을 알아봅시다.

```javascript
// 변수를 선언 하는 방법

// function scope
var a = 10      // 일반적인 변수 선언 방법

// block scope    {} 안에서만 존재하는 변수
let b = 20      // 재할당 가능  [최근에는 사용률이 낮아지고 있다.]
const c = 30     // 재할당 불가능 ["재할당 불가능"이라는 설명은 !== 값이 바뀌지 않는다.]
```

​	

​	

##  Javascript는 크게 두 가지 타입으로 구분합니다.

>**원시타입** VS **참조타입**</br>원시 타입은 숫자(Number), 불린(Boolean), null, undefined, 문자열(String), 심볼 총 6가지가 존재하고</br>참조 타입은 객체(Object), 배열(Array), 함수(function)가 있습니다.

​	

### 1. 원시 타입(Primitive Data Type)

>원시 타입의 데이터는 변수에 할당이 될 때 메모리 상에 고정된 크기로 저장이 되고 </br>해당 변수가 원시 **데이터 값을 보관합니다.** 원시 타입 자료형은 모두 변수 선언, 초기화, 할당 시 </br>**값이 저장된 메모리 영역에 직접적으로 접근합니다.** </br>즉, 변수에 새로운 값이 할당이 될 경우, **변수에 할당된 메모리 블럭에 저장된 값이 바로 변경됩니다.**

- 숫자형(Number)

  ```javascript
  // Number
  const a = 13
  const b = -5
  const c = 3.14 // float
  const d = 2.00e8 // 2.99 * 10**8
  const e = Infinity
  const f = -Infinity
  const g = NaN // Not a Number  하지만 타입은 넘버...
  
  // 변수의 타입을 알고 싶을 때
  console.log(typeof(g))
  typeof(g) // 개발자 도구 창에서는 typeof(g) 만 입력해도 보임.
  ```

  

- 불린형(참, 거짓)(Boolean)

  ```javascript
  // Boolean (소문자로 작성)
  true
  false
  
  // false 값을 초기값으로 변수 선언
  var bNoParam = new Boolean();
  var bZero = new Boolean(0);
  var bNull = new Boolean(null);
  var bEmptyString = new Boolean('');
  var bfalse = new Boolean(false);
  
  // true 값을 초기값으로 변수 선언
  var btrue = new Boolean(true);
  var btrueString = new Boolean('true');
  var bfalseString = new Boolean('false');
  var bSuLin = new Boolean('Su Lin');
  var bArrayProto = new Boolean([]);
  var bObjProto = new Boolean({});
  ```

  

- 문자형(String)

  ```javascript
  // String
  const sentence1 = 'Ask and go to the blue'
  const sentence2 = 'Ask and go to the red'
  
  const firstName = 'Tony'
  const lastName = 'Stark'
  const fullname = firstName + lastName
  console.log(fullname)  // expected output: TonyStark
  
  // 줄바꿈
  const word = '안녕 \n하세요'		//  \ => 달러표시
  console.log(word)
  
  // ✨String = Temlates Literal✨
  // `을 활용해 여러 줄에 걸처 문자를 정의 할 수도 있고
  // JS의 변수를 문자열 안에 바로 연결할 수 있는 이점이 생긴다.
  
  const ssafy = '5반'
  const word3 = `${ssafy} 안녕들 하신가`
  ```

  

- Null & undefined (Empty Value) 타입

  ```javascript
  // Empty Value (null & undefiened)
  // 값이 존재하지 않음을 표현하는 값으로  null과 undefiend가 있다.
  // 두 개가 존재하는 이유는 단순한 설계 실수로 인함 때문이다.
  
  let firstName
  console.log(firstName)  // 변수 값을 지정하지 않으면, undefined 출력됨.
  
  let lastName = null
  console.log(lastName)  // null
  
  // null과 undefined의 차이
  // null 또는 undefined를 검사할 때, 동등 연산자(==)와 일치 연산자(===)의 차이를 주의하세요. 
  // 동등 연산자는 자료형 변환을 수행합니다.
  
  typeof null          // "object" (하위호환 유지를 위해 "null"이 아님)
  typeof undefined     // "undefined"
  null === undefined   // false
  null == undefined   // true
  null === null        // true
  null == null         // true
  !null                // true
  isNaN(1 + null)      // false
  isNaN(1 + undefined) // true
  ```



- 심볼형(Symbol)

  ```javascript
  // 심볼 자료형은 ES6부터 추가된 원시 자료형입니다. 
  // 다른 원시형과 다르게(유일하게) 변경 불가능한 자료형으로, 
  // 참조형의 키(key)로도 사용이 가능합니다.
  ```

  ​	

### 2. 참조 타입(Reference Data Type)

>참조 타입의 데이터는 크기가 정해져 있지 않고 변수에 할당이 될 때</br>**값이 직접 해당 변수에 저장될 수 없으며 변수에는 데이터에 대한 참조만 저장됩니다.** 
>
>데이터에 대한 참조만 저장 ===  변수의 값이 저장된 힙 **메모리의 주소값을 저장한다.** === **변수가 가지고 있는 메모리 주소를 이용해서 변수의 값에 접근**한다.

- 객체

  - Object

    ```javascript
    // Object 객체 생성(선언)
    new Object()
    
    // 생성(생성자 함수 사용)
    const ob = new Object()
    console.log(ob)  //  ▪{} => __proto__: Object
    
    // 프로퍼티 추가
    ob.id = 'qqq'
    ob.pass = 1234
    ob.gender = "m"
    console.log(ob)  // {id: 'qqq', pass: 1234, gender: "m"}
    
    // 추가 생성법 및 프로퍼티 추가
     	// 방법.1
        const object1 = { a: 'foo', b: 42, c: {} };
        console.log(object1.a);  // "foo"
    
        // 방법.2
        const a = 'foo';
        const b = 42;
        const c = {};
        const object2 = { A: a, B: b, C: c };
        console.log(object2.B);  // 42
    
        // 방법.3 (방법.2에 이어서)
        const object3 = { a, b, c };
        console.log(object3.a);  // "foo"
    
        // 방법.4 (객체 리터럴 방식으로 생성)
    	var me = {id: 'JG1', pass: 1234, gender:'m'}
        console.log(me.id)  // "JG1"
    
    //////////////////////////////////////////////////////////////////////////////
    
    // 2. Object (python의 dict와 비슷한듯.)
    // key는 문자열 타입이고, value는 모든 타입이 될 수 있다.
    
    const me = {
        name:'홍길동',  // key가 한 단어 일때
        'phone number':'01012345678',  // key가 여러 단어 일때
        samsungProducts: {
            galaxyWatch: '2019GalaxyWatch',
            galaxyPhone: 'galaxy S9',
            galaxyBuds: 'Buds V1',
        },
    }
    
    // 두가지 방법으로 접근이 가능
    // (1) dot notation
    me.name // 홍길동
    me.samsungProducts.galaxyBuds // "Buds V1"
    
    // (2) []로 호출
    me['phone number'] // '01012345678'
    
    
    // Object.keys() - O 대문자
    const fruits = {a:'apple', b:'banana'}
    Object.keys(fruits) // ["a", "b"]
    
    // Object.values()
    Object.values(fruits) // ["apple", "banana"]
    
    //Object.entries()
    Object.entries(fruits) // [ ["a", "apple"], ["b", "banana"] ]
    
    // Object Literal (ER6+)
    const books = ['Learning JS', 'Eloquent JS']
    const comics = {
        DC : ['Aquaman', 'SHAZAM'],
        Marvel: ['Captain Marvel', 'Avengers']
    }
    const megazines = null
    const bookStore = {
        books: books,
        comics: comics,
        megazines: megazines,
    }
    bookStore.books[0] // 'Learning JS'
    ```

    

  - 배열(Array)

    ```javascript
    // 1. Array
    const numbers = [1,2,3,4]
    numbers[0] // 1
    numbers[-1] // undefined / 정확한 양의 정수만 사용 가능
    numbers.length // 4
    
    //reverse - 원본 배열의 요소들의 순서를 반대로 정렬한다. 
    numbers.reverse // [4,3,2,1]
    numbers.reverse // [1,2,3,4]
    
    const strings = ['d','c','b','a']
    strings.reverse  // ['a','b','c','d']
    
    // push & pop - 요소를 배열의 가장 뒤에 추가하거나 삭제한다.
    numbers.push('a') // [1, 2, 3, 4, "a"]
    numbers.pop() // 'a'
    
    // unshift & shift - 요소를 배열의 가장 앞에 추가하거나, 제거한다.
    numbers.unshift('a') // ["a", 1, 2, 3, 4]
    numbers.shift() // 'a'
    
    // includes - 배열의 특정 요소가 있는지 여부를 boolean 값으로 반환해준다.
    numbers.includes(1) // true
    numbers.includes(0) // false
    
    // indexOf
    // 배열의 특정 요소가 있는지 여부를 확인 후, 
    // 가장 첫번째로 찾은 아이템의 index값을 반환한다.
    // 요소가 없으면 -1을 반환
    numbers.push('a','a')
    numbers.indexOf('a') // 4
    numbers.indexOf('b') // -1
    ```

    

  - 함수(function)

    ```javascript
    // 함수 선언은 크게 두 가지로 나누어서 생각해볼 수 있다. 
    
    // 1. Function Scope - var를 사용할 때
    // python의 global 선언한 def 속 변수 느낌..
    
    function number(num) {
        var acc = num + 1
        return acc
    }
    console.log(acc) // num이 정의 되지 않아서 error
    
    if (true) {
        var name = '용우와 영수'
    }
    console.log(name) // '용우와 영수'
    
    
    // 2. Block Scope - const, let을 사용할 때
    // {} 안에서만 존재하는 변수
    // python의 global 선언 안한 def 속 변수 느낌..
    
    let age = '30'
    if (true) {
        let age = '100'
        console.log(age) // 100
    }
    console.log(age) // 30
    
    function numberTwo(num) {
        let accTwo = num +1
        return accTwo
    }
    console.log(numberTwo(2)) // 3
    ```

    ​	

    ​		

    ​	

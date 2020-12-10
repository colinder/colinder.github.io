# Javascript Operators


​	

# Javascript Operators

>Javascript의 연산자를 알아 봅시다. </br>종류가 많이 있어 많이 사용하는 것들 중심으로 먼저 알아보겠습니다.

​	

### 1. 할당 연산자

```javascript
// 할당 연산자
let numder = 0
numder += 10
console.log(numder)  // 10

numder -= 8
console.log(numder)  // 2

numder *= 10
console.log(numder)  // 20

numder ++  // 1을 더한다.
console.log(numder)  // 21

numder --  // 1을 뺀다.
console.log(numder)  // 20

// 종류 더 많이 존재합니다!
```

​	

### 2. 비교 연산자

```javascript
// 비교 연산자
3 > 2 //true
3 < 2 //false

"A" < "B"  //true
'z' < 'a'  //false

'a'.codePointAt(0) // 97(등록된 절대값)

// 동등 연산자(==)   이는 절대로 쓰지 않는다.
// 메모리에 같은 객체를 가리키거나 
// 같은 값을 갖도록 type 변환할 수 있다면 서로 같다고 판단.

const a = 1
const b = "1"
a == b //true

console.log(8*null) // 0
console.log('5'-1) // 4
console.log('5'+1) // 51
console.log("five" * 2) // NaN

// 하여 일치 연산자(===)를 사용한다. type도 같아야 동일하다고 판단.
const a = 1
const b = "1"
a === b //false
```

​	

### 3. 논리 연산자

```javascript
// 논리연산자
// And 연산자: &&
true && false // false
true && true // true

1 && 0 // 0
0 && 1 // 0
4 && 7 // 7

// Or 연산자: ||
false || ture // true
false || false // false

1 || 0 // 1
0 || 1 // 1
4 || 7 // 4

// Not 연산자 !
!true // false
```

​	

### 4. 삼항(조건) 연산자

```javascript
// 삼항 연산자(Ternary Operator)
true ? 1 : 2    // 1     if문과 비슷. true 면 1, false 면 2
false ? 1 : 2    // 2

const result = math.PI > 4 ? 'Yep' : 'Nope'
console.log(result) // Nope

let age = 21
let message = age < 7 ? '애기입니다.' :
    age < 20 ? '청소년입니다. ' :
    age < 100 ? '어른입니다. ':
    '사람입니다.'
console.log(message)  // 어른입니다.
```

​	

더 많은 연산자들이 존재합니다. 더 알고 싶다면 [MDN공식문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators)를 찾아 봅시다!

​	

​	

​	



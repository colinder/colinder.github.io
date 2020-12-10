# Javascript Scope


​	

# Javascript Scope란?

> Scope [ skoʊp ]</br>**1** (무엇을 하거나 이룰 수 있는) 기회[여지/능력] (=potential)</br>**2 (주제조직활동 등이 다루는) 범위**</br>**3** 샅샅이[자세히] 살피다.
>
> 스코프는 "유효 범위"로써 번수에 매개변수가 어디까지 유효한지를 나타냅니다.
>
> 자바스크립트에선 스코프는 2가지 타입이 있습니다. 바로 <b>Global(전역)</b>과 <b>Local(지역)</b>인데요. </br>**함수 안에서 선언된 변수(Local 변수)는 함수 블록 안에서만 접근이 가능합니다.**</br>전역 변수는 어디서든 접근이 가능합니다.

```javascript
// global 변수 선언
var a = 10;
console.log("global a: ", a)    // 10

// local 변수 선언
function print() {
    var b = 20;
    if (true) {
        var c = 30;
    }

    console.log("local b: ",b)  // 20
    console.log("local c: ",c)  // 30
}
print()
console.log("local b: ",b)  // ReferenceError: b is not defined

// ✨ Point
// b는 print함수 안에서 선언된 local 변수 임으로 
// print함수가 동작하는 안에서는 존재하지만, 
// print함수 밖에서는 존재하지 않습니다.
```

​	

## 스코프는 렉시컬(Lexical)과 다이나믹(Dynamic)으로 분류.

렉시컬(Lexical) 스코프는 코드를 작성하는 시점에 스코프가 결정되어진다고 해서 **정적 스코프**라고도 부릅니다. Javascript는 대표적인 렉시컬 스코프입니다.

```javascript
var a = "Global"

function print1() {
    console.log(a);
}

function print2() {
    var a = "Local";
    print1();
}

print1()    // Global
print2()    // Global

// print2에서 a를 "Local"로 재할당 했지만,
// print1()이 시작되면서 a는 print2()를 벗어나게 되고
// a는 "Global"로 돌아가게된다.
```

​	

​	

​	



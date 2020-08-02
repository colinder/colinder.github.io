# Vue_basic_02 (Vue 문법2)


​	

## Vue의 기본 문법 및 동작 방법을 `계속` 알아보자

**08_methods**

```vue
✨Point
1. method는 methods: {}의 문법으로 구성한다.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Methods</title>
</head>
<body>
  <div id='app'>
    {{ message }}
  </div>
  <!-- CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

  <script>
    const app = new Vue({
      el: '#app', 
      data: {
        message: 'Hello Vue'
      },
      methods: {
        alertWarning: function() {
          alert('WARNING')
        },
        alertWarning () {   // Syntactic Sugar : 위와 완전히 동일
          alert(this.message)
        },
        changeMessage() {
          this.message = 'CHANGED MESSAGE'
        }
      }
    })
  </script>
</body>
</html>
```

​	

**09_v-on**

```vue
위에서 선언한 methods를 이제 실행한다. 

✨Point (아주아주 중요)
1. 난, v-on은 이벤트(methods 등)를 실행시켜주는 트리거라고 이해했다.
2. 트리거를 발동시키는 내용은 여러가지가 있으니 이는 별도로 공부해야 한다.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>v-on</title>
</head>
<body>
  <div id='app'>
    <h1>{{ message }}</h1>
    <button v-on:click="alertWarning">Alert warning</button>
    <button v-on:click="alertMessage">Alert Message</button>
    <button v-on:click="changeMessage">Change Message</button>
    <!-- v-on: 는 @ 로 축약해서 사용할 수 있다.  -->
    <button @click="changeMessage">Change Message</button>
    <hr>
    <input v-on:keyup.enter="onInputChange" type="text">
  </div>
  <!-- CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app', 
      data: {
        message: 'Hello Vue'
      },
      methods: {
        alertWarning: function() {
          alert('WARNING')
        },
        alertMessage () {   // Syntactic Sugar : 위와 완전히 동일
          alert(this.message)
        },
        changeMessage() {
          this.message = 'CHANGED MESSAGE'
        },
        onInputChange(event) {
          // console.log("!!")
          // if (event.key === 'Enter') {
            this.message = event.target.value
          // }
        },
      }
    })
  </script>
</body>
</html>
```

👍 v-on 이벤트 종류는 [여기](https://developer.mozilla.org/ko/docs/Web/Events)에 모두 있다. 

​	

**10_v-model**

```vue
✨Point
1. 난, data값과 input값을 실시간으로 연동(양방향 바인딩)해주는 디렉티브
2. v-on(@)과 value를 동시에 걸어주면 양방향 바인딩이 가능함.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>v-model</title>
</head>
<body>
  <div id="app">
    <h1>{{ message }}</h1>
    <!-- v-model은 input, select, textarea에만 사용이 가능-->
    <h4>단반향 binding (input => data)</h4>
    1way <input @keyup.enter="onInputChange" type="text">
    <hr>
    <h4>양방향 binding</h4>
    2way <input @keyup.enter="onInputChange" type="text" :value="message">
    <hr>
    <!-- 단뱡향과 양방향을 구분하기 귀찮아서 model을 개발 -->
    <h4>v-model 양방향 binding</h4>
    v-model/2way
    <input v-model="message" type="text">

  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el:'#app',
      data: {
        message: 'hi'
      },
      methods: {
        onInputChange(event) {
          // console.log("22")
          this.message = event.target.value
        }
      }
    })
  </script>
</body>
</html>
```

​	

**11_v-show**

```vue
✨Point
1. v-if와 거의 유사하나 개발자 도구로 찍어보면 v-if는 조건에 따라 렌더링이 있다 없다 하지만, v-show의 요소는 항상 렌더링 되고 DOM에 남아있다.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>v-show</title>
</head>
<body>
  <div id="app">
    <!-- v-if와 거의 유사!-->
    <button @click='changeF'>changeF</button>
    <p v-if="t">True</p>
    <p v-if="f">false</p>
    <p v-show="t">show True</p>
    <p v-show="f">show false</p>
  </div>
  
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: "#app",
      data: {
        t: true,
        f: false,
      },
      methods: {
        changeF() {
          this.f = !this.f

        }
      }
    })
  </script>
</body>
</html>
```

​	

**12_computed**

```vue
✨Point
1. computed는 페이지 시작시 별다른 선언없이도 자동으로 실행된다.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id='app'>
    <h1>Bankrruped</h1>
    {{ getBankrrupedPeople }}
  </div>
  <!-- CDN 설정 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app', 
      data: {
        accounts: [
          { name: 'neo', balance: 500, isBankrruped: true },
          { name: 'tak', balance: 700, isBankrruped: false },
          { name: 'john', balance: 350, isBankrruped: false },
          { name: 'justin', balance: 200, isBankrruped: true },
        ],
        bankrrupedPeople: []
      },
      // 함수인데, data를 create, Update, Delete 하지 않고, Read(return)하고 싶을 때.
      computed: {
        getBankrrupedPeople: function() {
          const newArr = this.accounts.filter((account) => {
            return account.isBankrruped
          })
          return newArr
        }
      }
    })
  </script>
</body>
</html>
```

​	

### 종합 실습(로또 번호 추천)

```vue
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>lotto</title>
</head>
<body>
  <div id="app">
    <button @click="getLuckySix">getLuckySix</button>
    <ul>
      <li v-for="number in myNumbers">
        {{ number }}
      </li>
    </ul>
  </div>

  <!-- 수학적 커멘드는 lodash 활용 -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.15/lodash.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: "#app",
      data: {
        allNumbers: _.range(1, 46),
        myNumbers: [] 
      },
      methods: {
        getLuckySix() {
          this.myNumbers = _.sampleSize(this.allNumbers, 6)
          // vue는 str로 자료를 보는데 이를 정수로 바꾸기 위한 코드는 아래와 같다.
          // vue에서 str과 str을 빼는 건 int로 값이 나오기 때문에
          this.myNumbers.sort((a, b) => a - b)
        }
      },
    })

  </script>
</body>
</html>
```


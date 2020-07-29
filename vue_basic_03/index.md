# Vue_basic_03


​	

## 1. [VUE CLI 란?](https://simplevue.gitbook.io/intro/01.-vue-cli) (공식문서)

간단히 vue-cli 는 기본 vue 개발 환경을 설정해주는 도구

> **여기서 CLI 란 ?**
>
> **명령 줄 인터페이스(CLI, Command line interface) 또는 명령어 인터페이스**는 텍스트 터미널을 통해 사용자와 컴퓨터가 상호 작용하는 방식. 즉, 작업 명령은 사용자가 컴퓨터 키보드 등을 통해 문자열의 형태로 입력하며, 컴퓨터로부터의 출력 역시 문자열의 형태로 주어진다. (위키백과)

​	

## 2. vue router

프로젝트 생성 후 보통의 경우 편의를 위해 terminal에 vue add router를 입력해 router설치를 진행한다. 

router를 설치하고 나면 기존의 vue 프로젝트의 **tree구조**에서 약간의 변화가 생긴다. 

> router 설치 전: App.vue - components
>
> router 설치 후: App.vue - **views** - components

​	

### views 폴더 속 요소

1. views안의 .vue 파일들은 App.vue에 직접적으로 연결되며, component의 내용을 담는 것도 가능하다. 

   #### 🤷‍♂️ 실습해보자

   - 부모 views에 자식 component를 담는 상황을 가정하고 코딩 (Step 1~3 으로 구성)

     #### Step 1.

     ```vue
     ## App.vue
     
     <template>
       <div id="app">
         <div id="nav">
           <!-- Component에 등록한 name 사용법 (위 아래가 동일) -->
           <router-link :to="{ name: 'Parent' }">Parnet</router-link> |
           <!-- 기존 사용법 -->
           <router-link to="/about">About</router-link>
         </div>
         <!-- 연결된 router의 내용을 보여주는 곳 -->
         <router-view/>  
       </div>
     </template>
     
     <style>...
         
     ## 현재 2개의 router-link가 걸려있으며. 아래 <router-view/> 부분이 링크 선택시 연결된 링크의 화면을 보여주는 부분
     ```

     {{<image src="/images/image-20200716112330564.png" caption="`npm run serve`시 보여지는 화면" width="800px">}}

     #### Step 2.

     ```vue
     ## components/Child.vue (직접 생성)
     
     <template>
       <div class="child">
         <h2>자식 컴포넌트</h2>
       </div>
     </template>
     
     <script>
     export default { 
       name: 'Child',
     }
     </script>
     
     <!-- views에서 components를 담을 때 모습이 어떻게 담기는기 보기 위해 경계선을 구현 -->
     <style>
       .child {
         border: 3px solid blue;
         margin: 3px;
         padding: 3px;
       }
     </style>
     ```

     ​	

     #### Step 3.

     ```vue
     ## views/Parent.vue (직접 생성)
     ## views에 components 등록
     
     ✨ Point! 
     1. views에서 components를 등록(Parent가 Child를 품고 싶다면)하고 싶다면, 
     	1) components의 <script></script>에 import 진행
     	2) <script></script>안에 components: {Child}의 문법으로 등록
     	3) <template></template>안에 components이름으로 탭을 만들면 해당 위치에 components 표현 가능
     
     <template>
       <div class="parent">
         <h1>부모 컴포넌트</h1>
         <!-- step 3. 사용 (feat. 나중에 자식에게서도 데이터를 받음. this.$emit('hungry' 여기 값을 반영)) -->
         <Child/>
         <h2>최하단입니다.</h2>
       </div>
     </template>
     
     <script>
     // step 1. import
     import Child from '../components/Child.vue'
     
     export default {
       name: 'Parent',
       // step 2. 등록
       },
       components: {
         // 'Child': Child, 키 벨류가 같으면 아래와 같이 사용가능
         Child,
       }
     }
     </script>
     
     <!-- views에서 components를 담을 때 모습이 어떻게 담기는기 보기 위해 경계선을 구현 -->
     <style>
       .parent {
         border: 3px solid red;
         margin: 3px;
         padding: 3px;
       }
     </style>
     ```

     ​	

2. Vue 프로젝트를 생성해 기본적인 component등록 및 화면을 구성하다보면 각각의 component에서 상태 혹은 작성, 변경된 데이터를 이리저리 보낼 수 있다면 더 다양한 것을 할 수 있을 겠다는 생각이 들 수 있다. 이를 위한 것이 **props(부모에서 자식에게 데이터 이동)** 와 **emit(자식에서 부모에게 데이터 이동)** 이다. 

   #### 🤷‍♀️ 실습해보자

   - 부모 views에서 자식 component간의 용돈을 주고 받는 상황을 가정하고 코딩해보자 (Step 1~4)

     #### Step 1.

     ```vue
     ## App.vue
     
     <template>
       <div id="app">
         <div id="nav">
           <!-- Component에 등록한 name 사용법 (위 아래가 동일) -->
           <router-link :to="{ name: 'Parent' }">Parnet</router-link> |
           <!-- 기존 사용법 -->
           <router-link to="/about">About</router-link>
         </div>
         <!-- 연결된 router의 내용을 보여주는 곳 -->
         <router-view/>  
       </div>
     </template>
     
     <style>...
     ```

     #### Step 2.

     ```vue
     ## views/Parent.vue (직접 생성)
     ## 부모에서 자식에게 데이터 전달 (views → components)
     
     ✨ Point! 
     1. views에서 components를 등록(Parent가 Child를 품고 싶다면)하고 싶다면,
     	1) components의 <script></script>에 import 진행
     	2) <script></script>안에 components: {Child}의 문법으로 등록
     	3) <template></template>안에 components이름으로 탭을 만들면 해당 위치에 components 표현 가능
     2. views에서 components에 데이터를 넘길때에는,
     	1) <template></template>안에 components탭에 :내리고 싶은 데이터="데이터명" `:propFromParentMsg="parentMsg"`의 문법으로 전달
     
     <template>
       <div class="parent">
         <h1>부모 컴포넌트</h1>
         <!-- step 3. 사용  -->
         <Child :propFromParentMsg="parentMsg"/>
         <h2>최하단입니다.</h2>
       </div>
     </template>
     
     <script>
     // step 1. import
     import Child from '../components/Child.vue'
     
     export default {
       name: 'Parent',
       // step 2. 등록
       data () {
         return {
           parentMsg: '용돈 필요하니(부모가 전달한 메세지)'
         }
       },
       components: {
         // 'Child': Child, 키 벨류가 같으면 아래와 같이 사용가능
         Child,
       }
     }
     </script>
     
     <!-- views에서 components를 담을 때 모습이 어떻게 담기는기 보기 위해 경계선을 구현 -->
     <style>
       .parent {
         border: 3px solid red;
         margin: 3px;
         padding: 3px;
       }
     </style>
     ```

     #### Step 3.

     ```vue
     ## components/Child.vue (직접 생성)
     ## 1. views에서 components에 받은 데이터 노출 (부모에게서 받은 데이터 노출) 
     ## 2. components에서 views에 데이터 전달 (자식에서 부모에게 데이터 전달)
     
     ✨ Point! 
     1. views에서 받은 데이터를 노출하고 싶다면,
     	1) <script></script>에 전달받은 데이터의 자료형을 선언해주며 등록 
     		ex) props: {propFromParentMsg: String,}
     	2) 보간법('{{}}')을 사용하여 노출. ex) {{ propFromParentMsg }}
     
     2. components에서 views에 데이터를 넘길때에는 이벤트를 생성해 전달하는데,
     	1) 부모에게 전달한 메서드 만든다.
     		methods: {  moneySignal () {this.$emit('needMoney')}  } : 
     		📌 this.$emit() : 부모에게 전달하는 이벤트 이름 📌
     
     	2) 메서드를 작동시킬 이벤트를 만든다. 
     		ex) <button @click="moneySignal">용돈 필요해요!!!</button> 
     			→ 클릭시 moneySignal이라는 메서드 실행
     
     <template>
       <div class="child">
         <h2>자식 컴포넌트</h2>
         <!-- 부모에게서 받은 데이터 노출 -->
         {{ propFromParentMsg }}
         <br/>
         <button @click="moneySignal">용돈 필요해요!!!</button>
       </div>
     </template>
     
     <script>
     export default { 
       name: 'Child',
       // 부모에게서 받는 데이터 - props 등록 (반드시 Object를 써야지 유효성 검사 가능)
       // 자료형까진 써주야 한다!
       props: {
         propFromParentMsg: String,
       },
       methods: {
         moneySignal () {
           // 부모한테 이벤트(시그널) 방출
           this.$emit('needMoney')
         }
       }
     }
     </script>
     
     <!-- views에서 components를 담을 때 모습이 어떻게 담기는기 보기 위해 경계선을 구현 -->
     <style>
       .child {
         border: 3px solid blue;
         margin: 3px;
         padding: 3px;
       }
     </style>
     ```

     #### Step 4.

     ```vue
     ## views/Parent.vue (직접 생성)
     ## 자식에게서 받은 데이터를 부모 data에 반영 후 다시 자식에게 데이터 전달 
     	(components → views → components)
     
     <template>
       <div class="parent">
         <h1>부모 컴포넌트</h1>
         <!-- step 3. 사용 (feat. 자식에게서도 데이터를 받음. this.$emit('needMoney')) -->
         <Child @needMoney="parentMsg='용돈 받아라'" :propFromParentMsg="parentMsg"/>
         <h2>최하단입니다.</h2>
       </div>
     </template>
     
     
     ## 용돈필요하니?(부모)(:propFromParentMsg="parentMsg") 
     	→ 용돈필요해요!(자식)(this.$emit('needMoney') 
     	→ needMoney이벤트 접수되면 부모 메세지 바꿔서 다시 전달
     	  (<Child @needMoney="parentMsg='용돈 받아라'" :propFromParentMsg="parentMsg"/>)
     ```

     {{<image src="/images/image-20200716113807810.png" caption="정상 작동 화면" width="800px">}}


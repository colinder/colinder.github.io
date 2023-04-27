# React_init


​	

# React 시작하기

> vue2 서비스가 23년 12월 31일에 종료된다... vue3는 개인적으로 실패한 프로젝트라 생각해 vue2를 고집하고 있었는데....
>
> vue3로 갈 것인가. 그냥 react로 갈 것인가 고민하다. 일단 알아보자는 마음으로 정리한 내용이다.

​		

​	

## react 시작하기 앞서 준비할 것

1. node.js
2. 에디터(ex. vscode)

​	

​	

## react 시작하기

> 준비가 끝났으니, react 프로젝트를 생성해보자

​		

1. 프로젝트 초기 세팅 명령어 

   ```react
   npx create-react-app hello-world
   ```

   - `hello-world` 라는 이름의 프로젝트를 생성

     단!. 저는 별도의 package를 설치하지 않아서

     <image src="/images/init_setting.assets/image-20230405111120417.png" width="auto" style="display: block;">

     package를 설치하겠나고 물어봤는데 당연히 Y 입니다. 

​		

2. 설치 완료 확인

   <image src="/images/init_setting.assets/image-20230405111902281.png" width="auto" style="display: block;">

   내용을 보니 <b style="color:blue">react</b>라는 것과 react-dom 그리고 react-script가 설치된 것을 볼 수 있습니다. 

   모두 중요한 요소입니다. 

​			

3. 실행하기

   ```react
   cd hello-world
   npm start
   ```

   <image src="/images/init_setting.assets/image-20230405122109858.png" width="auto" style="display: block;">

   정상적으로 설치 및 실행이 된 것을 확인하였습니다. 

   

   ​	

   ​	

   ​	

   ​	

   ​	
   
   

## Typescript 적용한 프로젝트 생성 방법

```bash
npx create-react-app hello-world --template typescript
```

- `hello-world` 라는 이름의 프로젝트를 생성하는데 typescript로 생성

​	

​	

​	

​	

​	



# React Scoped_css


​	

# React_Style 관리

> Vue에는 .vue 파일마다 `<style>`가 있고 여기에 `<style scoped>`를 주면 해당 파일에만 적용되는 style을 구성할 수 있는데, 
> react는 어떻게 하는지 알아봅니다.

​	

​	

## scoped css 설정

> 지역적으로 사용하고 싶은 css 설정 방법

​	

폴더 구조를 보자면. (구조는 마음대로 구성하여도 됩니다. 아래는 제 마음대로 구성한 것입니다.)

```bash
src
├── assets
│    ├── Styles
│        ├── common.css
│   ...
├── types
│   ├── global.d.ts
│   ...
└── pages
│   ├── test.tsx
│   ├── test.module.css
└── tsconfig.json
```

​		

​	

### ✨ CRA로 프로젝트를 생성한 것을 기준으로 `[name].module.css`파일을 생성하는 것으로 지역적인 css를 설정할 수 있습니다.

---

1. `types > global.d.ts`파일을 생성하고 이곳에 `[name].module.css`를 선언해줍니다. (react typescript에게 알려주는 느낌.)

   CRA(Create-React-App) 공식문서를 따르면 CRA로 프로젝트를 생성했을 때, 별도의 설정없어 `[name].module.css`가 적용된다고 [하는데](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet/) 저는 붉은 경고창을 보았습니다. 그래서 등록해줬습니다. [참고](https://velog.io/@towozy/tsx%EC%97%90%EC%84%9C-.module.scss-import-%EB%AA%BB%ED%95%A0-%EB%95%8C)

   ```react
   declare module "*.module.css" {
     const classes: { [className: string]: string };
     export default classes;
   }
   
   //or
   
   declare module "*.module.css"
   ```

   ​	

2. types를 선언해줬기 때문에 이를 default로 참고하라고 `tsconfig.json`에 등록해줍니다.

   ```json
   {
     "compilerOptions": {
       "typeRoots": ["./node_modules/@types", "types"],  // 👈👈👈
   }
   ```

   ​	

3. 우선 `pages > test.tsx`에 기본 구조를 만들고

   ```react
   function test() {
     return (
       <div >
         <h1>test 메인 페이지</h1>      
         <div>여기를 바꿔 봅시다.</div>
       </div>
     );
   }
   
   export default test;
   ```

   ​	

4. `pages > test.module.css`에 test.tsx에만 적용할(지역적으로) css를 구성합니다. 간단하게 배경색을 주고

   ```react
   .Box {
     background-color: aqua;
   }
   ```

   ​	

5. `pages > test.tsx`에 반영하면 됩니다. 

   ```react
   import styles from "./test.module.css";		 // 👈👈👈
   
   function test() {
     return (
       <div >
         <h1>test 메인 페이지</h1>      
         <div className={styles.Box}>여기를 바꿔 봅시다.</div>		 // 👈👈👈
       </div>
     );
   }
   
   export default test;
   ```

​	

​	

​	

# 👀 참고사항

1. `global`로 .box 클래스를 설정하고 `.module.css`에도 .box 클래스를 설정하는 경우 `.module.css`가 적용됩니다.

2. 프로젝트가 커지는 경우 클래스명이 충돌될 수도 있는데, 기본적으로 `.module.css`이러한 충돌을 막기위해 CRA에서 구성된 것입니다.

3. 저는 CRA 설명대로 했는데도 경고가 떠서, 1, 2번의 과정이 추가되었습니다. 보다 정확한 방법은 공식문서를 참고하세요!

   ​	

   ​	

   ​	

   ​	

    


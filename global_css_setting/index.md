# React global_Style_관리


​	

# React Style_관리

> 전역으로 사용할 css 설정은 어떻게 하는지 알아봅니다.

*기본적으로 typescript가 적용되어 있습니다. 

​	

​	

## Global css 설정

> 전역으로 사용하고 싶은 css 설정 방법

​	

폴더 구조를 보자면. (구조는 마음대로 구성하여도 됩니다. 아래는 제 마음대로 구성한 것입니다.)

```bash
src
├── assets
│    ├── Styles
│        ├── common.css
│   ...
└── index.tsx
```

​	

1. global css 만들기

   저는 공통으로 사용할 css 요소를 `assets > Styles > common.css`에 구성하였습니다. 예를 들어

   ```react
   /* common.css */
   .red {
     background-color: red;
   }
   ```

   배경색을 붉게 하는 `red`라는 클래스를 만들었습니다.

   ​	

   ​	

2. 최상단 컴포넌트에 반영하기

   저는 index.tsx를 최상위 컴포넌트로 사용하기 때문에 여기에 등록(import)해줍니다. 

   ```react
   /* index.tsx */
   
   import React from 'react';
   import ReactDOM from 'react-dom/client';
   import App from './App';
   import './assets/Styles/common.css'	   /* 👈👈👈 */
   
   const root = ReactDOM.createRoot(
     document.getElementById('root') as HTMLElement
   );
   root.render(
     <React.StrictMode>
       <App />
     </React.StrictMode>
   );
   ```

   ​	

   ​	

   ​	

이렇게 등록해주면 어디서든 `red`라는 클래스를 사용할 수 있습니다. 	

​	

​	



​	


# react-native Things


​	

# React Native 시작하기 (with Typescript)

- myApp은 본인이 원하는 프로젝트 이름으로 바꾸면 됩니다.
- 가장 최근 버전으로 프로젝트 세팅

```bash
npx react-native@latest init myApp --template react-native-template-typescript
```

​	

​	

​	

## 실행명령어

Metro 실행 명령어

```bash
npx react-native start
```

​	

​	

​	

## 절대 경로 세팅

- [공식문서](https://reactnative.dev/docs/typescript#using-custom-path-aliases-with-typescript) 여기 참고하시면 좋습니다.  <b>v0.72 기준</b>

- 저는 아래와 같은 폴더 구조로 세팅했고 이 중 `src`를 root로 설정하고 싶었습니다.

- 또 @로 root 주소를 불러오고 싶습니다.

  ```bash
  ├─.bundle
  ├─android
  ├─ios
  ├─node_modules
  ├─src  👈               
  │  ├─assets
  │  │  ├─fonts
  │  │  └─styles
  │  ├─components
  │  ├─hooks
  │  ├─layout
  │  ├─screen
  │  │  └─intro
  │  │      ├─intro-calender
  │  │      └─intro-period
  │  └─utils
  ├─__tests__
  ├─package.json
  ├─tsconfig.json
  ├─babel.config
  ├ ...
  ├ ...
  
  ```

  ​	

​				

1. babel-plugin-module-resolver 설치하기

   ```bash
   npm i babel-plugin-module-resolver
   ```

​			

​				

2. babel.config.js에서 alias(별칭) 설정하기

   프로젝트를 생성하면 기본적으로 아래와 같이 설정되어 있습니다. 

   ```bash
   module.exports = {
     presets: ['module:metro-react-native-babel-preset'],
   };		
   ```

   ​	

   이렇게 바꿔 줍니다.

   ```bash
   module.exports = {
     presets: ['module:metro-react-native-babel-preset'],
     plugins: [
       [
         'module-resolver',
         {
           root: ['./src'],
           extensions: [
             '.ios.ts',
             '.android.ts',
             '.ts',
             '.ios.tsx',
             '.android.tsx',
             '.tsx',
             '.jsx',
             '.js',
             '.json',
           ],
           alias: {
             '@': './src',
           },
         },
       ],
     ],
   };
   ```

   ​	

3. tsconfig.json설정하기 (Typescript 사용시)

   defualt

   ```bash
   {
     "extends": "@tsconfig/react-native/tsconfig.json",
   }
   ```

   ​		

   아래와 같이 변경

   ```bash
   {
     "extends": "@tsconfig/react-native/tsconfig.json",
     "compilerOptions": {
       "baseUrl": "./src",
       "paths": {
         "@/*": ["./*"],
       }
     }
   }
   ```

### 끝

​	

​	

## Font 적용하기

Pretendard 폰트를 적용하고 싶었다. 

react-native는 web 처럼 css로 글로벌하게 지정하려면 라이브러리를 사용해야 했는데,

그게 싫어서 <customText> 를 만들어 사용하기로 생각했다. 

​			

- 알아둘 것
  - ios와 android 세팅 방법이 다르다. 난 android 기준으로 진행

​						

1. 원하는 폰트 다운로드 (otf나 ttf나 상관 없다. )

2. 적용할 폰트를 프로젝트 안에 넣는다. 여기서 android와 ios 각각 세팅하지않고 한번에 세팅되게 한다. 

   1. `root/src/assets/fonts` 폴더를 만들고 이 안에 otf 혹은 ttf 파일을 다 넣는다. 

      {{< image src="/images/things.assets/image-20230704174007654.png" width="400px" >}}

      - 여기서 ios 는 font name과 **PostScript name**이 다르면 정상적으로 동작되지 않는다하니, 이는 찾아보길 바란다.

3. root 주소에 `react-native.config.js`를 생성하고 아래와 같이 수정한다.

   {{< image src="/images/things.assets/image-20230704174025936.png" width="400px" >}}

   ```bash
   module.exports = {
     project: {
       ios: {},
       android: {}, 
     },
     assets: ['./src/assets/fonts'], //👈
   };
   ```

   👉 내가 폴더 구조를 `root/src/assets/fonts` 로 했기 때문에 이 구조를 등록한 것인다. 변경하고 싶다면, 마음대로 변경해도 된다. 

   ​			

4. 명령어를 입력해 ios, android 폴더에 폰트를 반영시킨다!

   ```bash
   npx react-native-asset
   ```

   잘 되었는지 확인!

   - android
     - `android/app/src/main/assets/fonts` 가 생기며 이안에 폰트파일들이 잘 들어있는지 확인!
   - ios
     - `ios/{projectName}/Info.plist` 가 수정되었으며 `<key>UIAppFonts</key>` 아래에 폰트이름리스트가 잘 있는지 확인!

​				

5. 이제 적용해보자

   ```bash
   //App.tsx
   
   import React from 'react';
   
   import {
     StyleSheet,
     View,
     Text,
   } from 'react-native';
   
   function App(): JSX.Element {
     return (
       <View>
         <Text style={styles.highlight}>when i wasunger.설</Text>
       </View>
     );
   }
   
   const styles = StyleSheet.create({
     highlight: {
       fontFamily: 'Pretendard-Bold',    
     },
   });
   
   export default App;
   ```

​			

​			

6. <b>제일 중요합니다. 에뮬레이터를 완전히 껐다가 다시 켜보세요.</b>
   - 전 이걸 안했다가 적용이 안되서 구글 정독했고, 4시간을 버렸습니다.....
   - vscode와 android 에뮬레이터를 전부 끄고 다시 vscode를 실행하고 `npm run android`를 입력해 실행하였습니다. 
   - 그러니 정상적으로 폰트가 적용된 것을 확인했습니다. 

​	

​	

​		

​		

​		

## Navigation

// 스텍에 쌓고 이동하기 ( 뒤로가기시 페이지로 돌아옴 )

```bash
navigation.navigate('스크린 이름 ')
```

 

// 이전페이지로 돌아가기 ( 위의 navigate 를 이용하여 이동한 경우 가능 payLoad 가 2개 이상인 경우에 가능 하다 ) 

```bash 
navigation.pop();
```

 

// 스텍의 제일 윗 페이지로 이동

```bash 
navigation.popToTop()
```



 // 새롭게 컴포넌트를 스텍이 쌓는다. ( 이전에 스텍에 쌓여있으면 그걸 불러온다 )

```bash
navigation.push();
```

 

// 스텍쌓지 않고 이동하기 ( 뒤로가기시 이전페이지로 돌아갈 수 없음 )

```bash
navigation.replace('스크린이름')
```

  

// 중첩된 네비게이터에서 이동

```bash 
navigation.navigate('A_Screen', { screen: 'A_1Screen' })
```

  

// 데이터 전달 

```bash
navigation.navigate('Inquiry_R',{ 키값: 데이터 })
```

  

// 뒤로가기 

```bash
import { CommonActions } from "@react-navigation/native";

navigation.dispatch(CommonActions.goBack());

//or

navigation.goBack()
```

​	

​	

​	

​	


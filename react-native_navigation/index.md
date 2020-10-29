# React Native_navigation


​	

# React-Native navigation

> vue는 router를 사용해 화면을 이동합니다. 그렇다면, react-native는 어떻게 화면을 이동할까요?
>
> 바로 `react-native navigation`을 사용합니다. 어떻게 사용하는건지 기록합시다.

*공식홈페이지의 [Getting started](https://reactnavigation.org/docs/getting-started)로 알아봅시다.*

​	

*새로운 프로젝트를 만들고 root폴더에서 진행. 

- 개발환경

  - npm으로 진행

  - react-native_cli (✨expo로 진행하지 않습니다.)

  - android 기준

  - 함수형 컴포넌트로 진행 (✨ 클래스 컴포넌트로 진행하지 않습니다.)

  - RN 버전 0.60이상에서 진행

    - react-native-cli: 2.0.1

    - react-native: 0.63.3

      ​		

1. Getting started의 npm설치

   <image src="/images/RN_navigation_00.png" width="auto" style="display: block; margin: 10px auto;">

   ```bash
   npm install @react-navigation/native
   ```

   ​	

2. 화면을 조금 내려서 dependency도 설치

   <image src="/images/RN_navigation_01.png" width="auto" style="display: block; margin: 10px auto;">

   ```bash
   npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
   ```

   ​	

3. entry file파일에 import 등록

   <image src="/images/RN_navigation_02.png" width="auto" style="display: block; margin: 10px auto;">

   제가 개발하는 당시 기준 index.js를 보면 

   <image src="/images/RN_navigation_03.png" width="auto" style="display: block; margin: 10px auto;">

   9번 줄에 App에서 시작이 된다고 명시되어 있음으로 

   <image src="/images/RN_navigation_04.png" width="auto" style="display: block; margin: 10px auto;">

   App.js (a.k.a entry file) 최상단에 import를 등록합니다.

   ```bash
   import 'react-native-gesture-handler';
   ```

   *기본적인 사용환경 설정은 완료되었습니다. 다만, `stack`이라는 기술을 사용할 예정이라 추가로 진행할 것이 있습니다. 

   ​	

4. 공식문서의 [stack navigator library](https://reactnavigation.org/docs/hello-react-navigation)로 이동 후 설치

   <image src="/images/RN_navigation_05.png" width="auto" style="display: block; margin: 10px auto;">

   ```bash
   npm install @react-navigation/stack
   ```

   *이제 정말 개발 환경 설정이 완료되었습니다. 

   ​		

## React-Native navigation 사용해보자

> 페이지를 이동하는 것과 페이지를 이동할 때 params(인자)를 같이 넘겨주는 방법을 기록합니다.

먼저, root폴더에 src라는 폴더를 만들고 / home.js와 user.js 파일을 만듭니다. (함수형으로 개발하니 rnfe + tab을 입력해 양식을 빠르게 불러올 수 있습니다.) 틀을 잡고 시작합니다.

<image src="/images/RN_navigation_06.png" width="auto" style="display: block; margin: 10px auto;">

​	

### 페이지 이동(RN navigation)

> import → entry file 코딩→ components 코딩 순서로 진행합니다.

1. 설치한 navigation과 stack을 import 해줍니다.

   ```bash
   # App.js
   
   import 'react-native-gesture-handler';
   import React from 'react'
   import { NavigationContainer} from '@react-navigation/native'		//👈
   import { createStackNavigator} from '@react-navigation/stack'		//👈
   ...
   
   const Stack = createStackNavigator();  //👈 추가로 stack을 사용하기 위해 변수 선언
   ```

   ​			

2. Usage

   ```bash
   # App.js
   
   import 'react-native-gesture-handler';
   import React from 'react'
   import { NavigationContainer} from '@react-navigation/native'
   import { createStackNavigator} from '@react-navigation/stack'
   import HomeScreen from './src/home'
   import UserScreen from './src/user'
   
   const Stack = createStackNavigator();
   
   const App = () => {
     return (
       <NavigationContainer>
         <Stack.Navigator initialRouteName='Home'>
           <Stack.Screen name='Home' component={HomeScreen}/>
           <Stack.Screen name='User' component={UserScreen}/>
         </Stack.Navigator>
       </NavigationContainer>
     )
   }
   export default App
   
   # NavigationContainer (Fragments같은 겁니다.)
   # initialRouteName: 초기 화면을 Home으로 설정했습니다.
   ```

   ​		

3. home.js와 user.js도 코딩합니다

   ```bash
   # home.js
   
   import 'react-native-gesture-handler';
   import React from 'react'
   import { Text, View, Button } from 'react-native'
   
   const HomeScreen = ({ navigation }) => { 		//👈 navigation을 등록
     return (
       <View>
         <Text>Home Screen</Text>
         <Button
           title="To User Screen"
           onPress={()=>{							//👈 onPress: 버튼이 눌리면 실행
             navigation.navigate('User')			//👈 이런 문법으로 사용
           }}
         />
       </View>
     )
   }
   
   export default HomeScreen
   ```

   ```bash
   # user.js
   
   import 'react-native-gesture-handler';
   import React from 'react'
   import { Text, View, Button } from 'react-native'
   
   const HomeScreen = ({ navigation }) => { 		// navigation을 등록
     return (
       <View>
         <Text>Home Screen</Text>
         <Button
           title="To User Screen"
           onPress={()=>{
             navigation.navigate('Home')			// 이런 문법으로 사용
           }}
         />
       </View>
     )
   }
   
   export default UserScreen
   ```

   이 상태에서 시뮬레이터를 돌려보면 화면 전환이 되는 것을 볼 수 있습니다. But 할 것이 남았습니다. 이제 params도 전달 해봅시다.

   ​			

### 페이지 이동 with params 

> navigation.navigate(A, B)는 두개의 인자를 받습니다. A는 이동할 컴포넌트의 '이름', B가 '`params`'입니다. (아래는 home.js → user.js로 params를 보내는 예제입니다.)	

​					

1. home.js에서 전달할 params를 구성합니다.

   ```bash
   # home.js
   
   import 'react-native-gesture-handler';
   import React from 'react'
   import { Text, View, Button } from 'react-native'
   
   const HomeScreen = ({ navigation }) => { 
     return (
       <View style={{        // 화면에 보기 좋게 간단한 스타일 설정
         flex: 1, 
         alignItems: 'center', 
         justifyContent: 'center'
         }}>
         <Text>Home Screen</Text>
         <Button
           title="To User Screen"
           onPress={()=>{
             navigation.navigate('User', {       // 두번째 인자 입력
               userIdx: 100,
               userName: 'jong',
               userLastName: null,
             })
           }}
         />
       </View>
     )
   }
   export default HomeScreen
   ```

   ​				

2. user.js에서 parmas를 받습니다. 

   ```bash
   # user.js
   
   import 'react-native-gesture-handler';
   import React from 'react'
   import { Text, View, Button } from 'react-native'
   
   const UserScreen = ({ route, navigation }) => {				// route를 인자로 받음.
     const { userIdx, userName, userLastName } = route.params;  // 전달받은 값(route)
   
     return (
       <View>
         <Text>User Screen</Text>
         <Button
           title="To Home Screen"
           onPress={() => {
             navigation.navigate('Home')
           }}
         />
         <Text>User Idx: {JSON.stringify(userIdx)}</Text>		  {/* JSON의 string으로 받습니다. */}
         <Text>User Name: {JSON.stringify(userName)}</Text>
         { route.params.userLastName && <Text>User LastName: {JSON.stringify(userLastName)}</Text>}			{/* 조건을 줘봤습니다. {인자가 있을 때만 <Text>로 값을 표시} */}
       </View>
     )
   }
   export default UserScreen
   ```

   ​			

*결과 화면

| home.js                                                      | user.js                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <image src="/images/RN_navigation_07.png" width="auto" style="display: block; margin: 10px auto;"> | <image src="/images/RN_navigation_08.png" width="auto" style="display: block; margin: 10px auto;"> |

​		

​		



# 👀요약

> 지금까지 components간의 이동과 params, 간단한 스타일의 적용을 알아봤습니다. 해보면 어렵지 않으나, 모르면 다른 세상 이야기같습니다.
>
> 다음은 react-native에 google login을 심어보겠습니다.

​	



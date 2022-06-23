# React-Native_icon_and_splash


​	

# React-native (android)

> icon 변경과 splash를 적용해보겠습니다. 

<p style="text-align: right">icon: 핸드폰에서 어플 접속시 보이는 모습을 의미.</span><br>splash: 어플 실행시 잠시 노출되는 화면을 의미.

​	

​	

## Icon 변경 방법

> **결론부터 정리하자면 특정 폴더안에 이미지만 넣으면 됩니다.**

​	

*프로젝트를 시작할 때 이름을 **test**로 만들어 진행하였습니다. (버전은 0.67)

```javascript
$ npx react-native init test --version 0.67.0
```

​			

#### 1. android/app/src/main/res 까지 폴더를 들어갑니다. 

<image src="/images/react-native_icon_and_splash.assets/image-20220622143357001.png" width="auto" style="display: block; margin: 10px ;  float:left;">

<image src="/images/react-native_icon_and_splash.assets/image-20220622143452823.png" width="auto" style="display: block; margin-top: 15px; padding-top: 10px;"><br>

이미지와 같은 구조가 보인다면 잘 접근한 것입니다. 

그리고 오른쪽에 범위 잡혀있는 <b><span style="color:red">모든</span> 폴더 속의 이미지를 변경하고 싶은 이미지로 바꾸어 넣으면 됩니다.</b>

​	

​	

#### 2 .mipmap-hdpi 기준으로 살펴본다면, 

<image src="/images/react-native_icon_and_splash.assets/image-20220622143603188.png" width="auto" style="display: block; margin: 10px;">

<b>ic_launcher_round.png</b> 와 <b>ic_launcher.png</b> 를 내가 원하는 이미지로 변경.

​	

​	

#### 3. 이미지는 변환이 필요한데 1024px x 1024px 이미지로 변환하여 제작. <span style="font-size:0.6em">(반드시 해당 사이즈로 해야 하는지는 모르겠습니다.)</span>

한 번에 두 이미지를 만들어 주는 사이트는 찾지 못해 두 개의 사이트를 이용하여 이미지를 제작하였습니다.

- [ic_launcher_round.png 이미지 생성 사이트](http://romannurik.github.io/AndroidAssetStudio/icons-launcher.html#foreground.type=image&foreground.space.trim=1&foreground.space.pad=-0.1&foreColor=rgba(96%2C%20125%2C%20139%2C%200)&backColor=rgb(255%2C%20255%2C%20255)&crop=0&backgroundShape=circle&effects=none&name=ic_launcher)

- [ic_launcher.png 생성 사이트](https://appicon.co/)

​	

​		

#### 4. 제작한 두 이미지를 기존의 파일명과 동일하게 변경하고 덮어씌워 넣기

​	

​	

#### 5. 캐시 초기화 하기

- 아래 명령어를 통해 캐시를 초기화 합니다. <span style="font-size:0.8em">(이유: 환경설정 및 새롭게 추가된 파일을 읽어오는데 문제없게 하기 위함)</span>

  ```bash
  # root 폴더 위치(package.json이 있는 위치)에서 명령어를 입력합니다.
  $ cd android		# android 폴더로 이동
  $ ./gradlew clean	# android 폴더 안에 gradlew을 초기화
  
  #이 둘을 묶어서 한 번에 실행하고 싶다면, cd android && ./gradlew clean 
  ```

  해준 후 

  ​	

​		

#### 6. 다시 root 폴더 위치로 이동해

```bash
cd ..  # android폴더 안에서 상위 폴더로 빠져나오기
```

​	

​	

#### 7. 프로젝트를 실행해 변경된 아이콘을 확인

```bash
npm run android  // 빌드 및 시뮬레이터 실행
```

실행하면 변환된 Icon 확인 가능.

​	

​	

​	

​	

​	

## splash 변경 방법

> **icon 변경보다는 복잡하지만, 어렵지 않습니다.**

​	

#### 1. 관련 라이브러리 설치

스플래시 이미지를 적용하기 위한 라이브러리를 설치합니다.

```bash
npm i react-native-splash-screen --save
```

​		

​	



#### 2. 환경 설정하기

1. android/app/src/main/java/com/**test**/MainActivity.java 파일을 열어 수정합니다.<br>
   <span style="font-size: 0.8em">(중간에 test는 프로젝트 생성할 당시 만든  프로젝트명)</span>

   ```react
   package com.test;
   
   import android.os.Bundle; // add
   import com.facebook.react.ReactActivity;
   
   // react-native-splash-screen >= 0.3.1 
   import org.devio.rn.splashscreen.SplashScreen; // here 
   
   public class MainActivity extends ReactActivity {
     @Override
     protected void onCreate(Bundle savedInstanceState) {
         SplashScreen.show(this);  // here 
         super.onCreate(savedInstanceState);
     }
   
     /**
      * Returns the name of the main component registered from JavaScript. This is used to schedule
      * rendering of the component.
      */
     @Override
     protected String getMainComponentName() {
       return "test";
     }
   }
   ```

   만약에 <b>react-native navigation</b>을 설치한 상태라면, 중간 "@Override" 부분에 코드 중복이 발생하는데 그냥 무시하셔도 됩니다.

​	

​	

#### 3. splash 화면 그리기

1. android/app/src/main/res에 layout 폴더를 생성합니다.

2. layout 폴더에 <b>launch_screen.xml</b> 라는 이름의 파일을 생성합니다.

3. 방금 만든 launch_screen.xml 파일에 아래 코드를 복사하여 붙여넣어 줍니다.

   ```html
   <?xml version="1.0" encoding="utf-8"?>
   <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="vertical" android:layout_width="match_parent"
       android:layout_height="match_parent">
       <ImageView android:layout_width="match_parent" android:layout_height="match_parent" android:src="@drawable/launch_screen" android:scaleType="centerCrop" />
   </RelativeLayout>
   ```

​	

​	

#### 4, 스플래시 이미지 만들기

이미지를 필요한 사이즈 별로 만들어야 합니다.단 기존에 사용할 이미지 1장<span style='font-size: 0.8em'>(권장 사이즈 1242px x 2208px)</span>은 있어야 합니다.

1. 가지고 있는 splash 파일에 이름을 <b>launch_screen.png로 변경</b>합니다.

2.  https://appicon.co/#image-sets 사이트에 접속 합니다.

3. 접속한 사이트에 이미지를 넣고 Generate 버튼을 눌러 Splash 이미지를 다운로드 합니다.

   <image src="/images/react-native_icon_and_splash.assets/img-16558782448792.png" width="auto" style="display: block; margin: 10px;">

   <image src="/images/react-native_icon_and_splash.assets/img-16558782448793.png" width="auto" style="display: block; margin: 10px;">

​		

​			

#### 5. 다운받은 파일을 압축을 해제 후 android/app/src/main/res/ 경로에 파일을 복붙

<image src="/images/react-native_icon_and_splash.assets/img-16558782448794.png" width="auto" style="display: block; margin: 10px;">

​		

​		

#### 6. 캐시 초기화 하기

- 아래 명령어를 통해 캐시를 초기화 합니다. <span style="font-size:0.8em">(이유: 환경설정 및 새롭게 추가된 파일을 읽어오는데 문제없게 하기 위함)</span>

  ```bash
  # root 폴더 위치(package.json이 있는 위치)에서 명령어를 입력합니다.
  $ cd android		# android 폴더로 이동
  $ ./gradlew clean	# android 폴더 안에 gradlew을 초기화
  
  #이 둘을 묶어서 한 번에 실행하고 싶다면, cd android && ./gradlew clean
  ```

  여기까지 진행하고 시뮬레이터에 화면을 띄워보면 splash 이미지가 등장은 하나 사라지지 않습니다. 하여

​	

​	

#### 7. splash 등장 시간 설정

1. App.js 파일 SplashScreen Import 하고

   ```react
   import React, {useEffect} from 'react';
   import {StyleSheet, Text, View} from 'react-native';
   import SplashScreen from 'react-native-splash-screen';  // here
   
   export default function App() {
     useEffect(() => {
       try {
         setTimeout(() => {
           SplashScreen.hide();
         }, 1000); // here 스플래시 노출 시간 1초
       } catch (e) {
         console.log(e.message);
       }
     });
   
     return (
       <View>
         <Text>스플래쉬 테스트</Text>
       </View>
     );
   }
   ```

   ​		

2. React Native 에뮬레이터를 실행하여 확인합니다.

   ```bash
   npm run android
   ```

​	

 		

​		

​	

이상 react-native에서 icon 및 splash 설정을 알아보았습니다.	

​	

​	


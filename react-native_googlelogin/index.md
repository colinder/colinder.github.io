# React-Native google login


​	

# React-Native google login 

> 서비스를 개발하다보면 로그인 기능이 새삼 대단해보입니다. 신경쓸 것도 많고, 하지만 이런 수고를 sns로그인 기능으로 대신한다면 편의성이 많이 높아집니다. 하여 해보겠습니다.

_*google login을 하기 위해 firebase(모바일 및 웹 애플리케이션 개발 플랫폼)을 사용합니다._



- 개발환경

  - npm으로 진행

  - react-native_cli (✨expo로 진행하지 않습니다.)

  - android 기준

  - 함수형 컴포넌트로 진행 (✨ 클래스 컴포넌트로 진행하지 않습니다.)

  - RN 버전 0.60이상에서 진행

    - react-native-cli: 2.0.1

    - react-native: 0.63.3

      ​	

1. googleSignin 프로젝트 생성 후 googleSignin.js 생성

   ​	

2. index.js 에서 시작앱 변경

   ```bash
   # index.js
   
   /**
    * @format
    */
   
   import {AppRegistry} from 'react-native';
   import App from './googleSignin';			// 변경
   import {name as appName} from './app.json';
   
   AppRegistry.registerComponent(appName, () => App);
   ```

   ​	

3. googleSignin.js 틀 잡기

   ```bash
   # googleSignin.js
   
   import React from 'react'
   import { View, Text } from 'react-native'
   
   const googleSignin = () => {
     return (
       <View>
         <Text>google Sign in</Text>
       </View>
     )
   }
   export default googleSignin
   ```

   ​	

4. [react native google login github](https://github.com/react-native-google-signin/google-signin) 이동 후 root폴더 에서 google-signin 설치

   <image src="/images/RN_googleLogin_00.png" width="auto" style="display: block; margin: 10px auto; border: 1px solid grey">

   ```bash
   npm install --save @react-native-community/google-signin
   ```

   ​		

5. [Andriod guide](https://github.com/react-native-google-signin/google-signin/blob/master/docs/android-guide.md)로 이동

   <image src="/images/RN_googleLogin_02.png" width="auto" style="display: block; margin: 10px auto; border: 1px solid grey">

   `google-services.json` 파일을 generate해야 한다고 합니다. 이걸 위해 `firebase`에 등록하러 갑시다.

   ​		

6. [firebase](https://console.firebase.google.com/u/0/) 이동

   1. 프로젝트 추가 (+)

      {{<youtube xgwz3U9htJY>}}

   2. Android 패키지 이름 추출

      *root/android/app/src/main/AndroidManifest.xml 에서 확인

      <image src="/images/RN_googleLogin_03.png" width="auto" style="display: block; margin: 10px auto;">

   3. sha1 추출

      ```bash
      # root/android/app으로 이동
      # terminal
      
      keytool -list -v -keystore debug.keystore -alias androiddebugkey -storepass android -keypass android
      ```

      <image src="/images/RN_googleLogin_04.png" width="auto" style="display: block; margin: 10px auto;">

   4. 다시 firebase에서 안드로이드 설정 선택

      <image src="/images/RN_googleLogin_05.png" width="auto" style="display: block; margin: 10px auto;">

   5. 앱 등록 진행 하다보면 `google-services.json`다운이 가능

      _*혹시 에러 뜨면. react native change package name 검색 후 변경 진행_

   6. `google-services.json`다운 후 root/android/app안에 넣기

      <image src="/images/RN_googleLogin_06.png" width="auto" style="display: block; margin: 10px auto;">

      ​			

7. 다시 [Andriod guide](https://github.com/react-native-google-signin/google-signin/blob/master/docs/android-guide.md)로 돌아와서 `2. Installation` 진행

   1. link the native module (난 RN >= 0.60니까 pass)

   2. Update `android/build.gradle` with

      ```bash
      buildscript {
          ext {
              buildToolsVersion = "27.0.3"
              minSdkVersion = 16
              compileSdkVersion = 27
              targetSdkVersion = 26
              supportLibVersion = "27.1.1"
              googlePlayServicesAuthVersion = "16.0.1" // <--- use this version or newer	👈(추가)
          }
      ...
          dependencies {
              classpath 'com.android.tools.build:gradle:3.1.2' // <--- use this version or newer	👈(default로 입력되어 있었음)
              classpath 'com.google.gms:google-services:4.1.0' // <--- use this version or newer	👈(추가)
          }
      ...
      allprojects {
          repositories {
              mavenLocal()
              google() // <--- make sure this is included	👈(default로 입력되어 있었음)
              jcenter()
              maven {
                  // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
                  url "$rootDir/../node_modules/react-native/android"
              }
          }
      }
      ```

   3. Update `android/app/build.gradle` with

      ```bash
      ...
      dependencies {
          implementation fileTree(dir: "libs", include: ["*.jar"])
          implementation "com.android.support:appcompat-v7:23.0.1"
          implementation "com.facebook.react:react-native:+"
          implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.0.0' // <-- add this; newer versions should work too 👈(default로 입력되어 있었음)
      }
      ...
      ...
      apply plugin: 'com.google.gms.google-services' // <--- 👈 this should be the last line
      ```

   4. 이후 내용은 link관련인데. 난 RN >= 0.60니까 pass

      ​	

8. 다시 [react native google login github](https://github.com/react-native-google-signin/google-signin)을 참고하며 진행

   1. GoogleSignin

      ```bash
      # googleSignin.js
      
      import { GoogleSignin, GoogleSigninButton, statusCodes } from '@react-native-community/google-signin';
      ```

      ```bash
      # googleSignin.js
      
      const googleSignin = () => {
        useEffect(() => {
          GoogleSignin.configure({
            scopes: ['https://www.googleapis.com/auth/drive.readonly'], // what API you want to access on behalf of the user, default is email and profile
            webClientId: '<요기는 firebase를 참고해서 입력합니다.>', // client ID of type WEB for your server (needed to verify user ID and offline access)
            offlineAccess: true, // if you want to access Google API on behalf of the user FROM YOUR SERVER
            // hostedDomain: '', // specifies a hosted domain restriction
            // loginHint: '', // [iOS] The user's ID, or email address, to be prefilled in the authentication UI if possible. [See docs here](https://developers.google.com/identity/sign-in/ios/api/interface_g_i_d_sign_in.html#a0a68c7504c31ab0b728432565f6e33fd)
            forceCodeForRefreshToken: true, // [Android] related to `serverAuthCode`, read the docs link below *.
            // accountName: '', // [Android] specifies an account name on the device that should be used
            // iosClientId: '<FROM DEVELOPER CONSOLE>', // [iOS] optional, if you want to specify the client ID of type iOS (otherwise, it is taken from GoogleService-Info.plist)
          });
        }, [])
      }
      
      export default googleSignin
      
      # 필요없는 항목들은 주석해줍니다.다만 webClientId는 firebase에서 확인하고 등록합니다.
      # useEffect: 리액트 컴포넌트가 렌더링 될 때마다 특정 작업을 수행하도록 설정 할 수 있는 Hook 입니다. 클래스형 컴포넌트의 componentDidMount 와 componentDidUpdate 를 합친 형태로 보아도 무방합니다.
      ```

      <image src="/images/RN_googleLogin_07.png" width="auto" style="display: block; margin: 10px auto;">

      이제 signin코드를 입력합시다.

      ```bash
      # googleSignin.js
      
      const signIn = async() => {
          try {
            await GoogleSignin.hasPlayServices();
            const userInfo = await GoogleSignin.signIn();
            console.log(userInfo)					//👈콘솔로 로그인 정보 찍어보자
            // this.setState({ userInfo });		//👈함수형으로 개발하여 이건 불필요
          } catch (error) {
            if (error.code === statusCodes.SIGN_IN_CANCELLED) {
              // user cancelled the login flow
            } else if (error.code === statusCodes.IN_PROGRESS) {
              // operation (e.g. sign in) is in progress already
            } else if (error.code === statusCodes.PLAY_SERVICES_NOT_AVAILABLE) {
              // play services not available or outdated
            } else {
              // some other error happened
            }
            console.log(error)					//👈콘솔로 에러내용 찍어보자
          }
        }
      ```

      *완성된 코드

      ```bash
      # googleSignin.js
      
      import React, { useState, useEffect } from 'react';
      import { View, Text, Image, } from 'react-native'
      import { GoogleSignin, GoogleSigninButton, statusCodes } from '@react-native-community/google-signin';
      
      
      const googleSignin = () => {
        useEffect(() => {
          GoogleSignin.configure({
            scopes: ['https://www.googleapis.com/auth/drive.readonly'], // what API you want to access on behalf of the user, default is email and profile
            webClientId: '<요기는 firebase를 참고해서 입력합니다.>', // client ID of type WEB for your server (needed to verify user ID and offline access)
            offlineAccess: true, // if you want to access Google API on behalf of the user FROM YOUR SERVER
            // hostedDomain: '', // specifies a hosted domain restriction
            // loginHint: '', // [iOS] The user's ID, or email address, to be prefilled in the authentication UI if possible. [See docs here](https://developers.google.com/identity/sign-in/ios/api/interface_g_i_d_sign_in.html#a0a68c7504c31ab0b728432565f6e33fd)
            forceCodeForRefreshToken: true, // [Android] related to `serverAuthCode`, read the docs link below *.
            // accountName: '', // [Android] specifies an account name on the device that should be used
            // iosClientId: '<FROM DEVELOPER CONSOLE>', // [iOS] optional, if you want to specify the client ID of type iOS (otherwise, it is taken from GoogleService-Info.plist)
          });
        }, [])
        
        const signIn = async() => {
          try {
            await GoogleSignin.hasPlayServices();
            const userInfo = await GoogleSignin.signIn();
            console.log(userInfo)
            // this.setState({ userInfo });
          } catch (error) {
            if (error.code === statusCodes.SIGN_IN_CANCELLED) {
              // user cancelled the login flow
            } else if (error.code === statusCodes.IN_PROGRESS) {
              // operation (e.g. sign in) is in progress already
            } else if (error.code === statusCodes.PLAY_SERVICES_NOT_AVAILABLE) {
              // play services not available or outdated
            } else {
              // some other error happened
            }
            console.log(error)
          }
        }
        return (
          <View>
            <Text>google Sign in</Text>
            <GoogleSigninButton
              style={{ width: 192, height: 48 }}
              size={GoogleSigninButton.Size.Wide}
              color={GoogleSigninButton.Color.Dark}
              onPress={signIn}
              />
          </View>
        )
      }
      
      export default googleSignin
      ```

      ​		

      ​	

# 👀요약

> google login을 위해서는 firebase를 사용하는 것이 간편한 방법입니다.
>
> 저는 구글로그인을 진행하면서 useState도 같이 공부하였는데 해당 내용은 제외하였습니다. 하지만, 같이 공부하게 되실 겁니다.ㅎ

​	

​	

## 참고 했던 글

- https://www.youtube.com/watch?v=A1Ai4sKk0jM
- https://medium.com/humanscape-tech/hooks-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-usestate-useeffect-811636d1035e





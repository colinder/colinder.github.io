# 00_Vue_setting


​	

# Vue

>Vue.js는 웹 애플리케이션의 사용자 인터페이스를 만들기 위해 사용하는 오픈 소스 프로그레시브 
>**자바스크립트 프레임워크**
>
>간단히, `온라인 홈페이지를 만드는 프로그램`입니다. 

​	

## 1. 시작하기

> > 'Vue' VS 'Django'
>
> 어느 정도 공부를 하면서 체득한 가장 큰 구분점은 "`Django는 App별로 응답을 구분`하고, `Vue는 Component별로 응답을 구분`한다."는 것입니다.
>
> 큰 차이가 없어보이고, 큰 의미가 없어보일 수 있으나 알아두면 좋습니다!

​				

- #### 💻 개발 환경 설정 

  1. [vscode](https://code.visualstudio.com/) 설치

  2. [nodeJS](https://nodejs.org/ko/) 설치

     ```python
     # 🤔 안정적인 버전 vs 최신 버전 
     # 상관없으나 개인적으로 안정적인 버전을 추천합니다.
     
     # 😀 설치가 잘 되었나 확인 방법!
     # terminal창(cmd) -> node -v (명령어 입력)
     # 버전 넘버가 보이면 정상 설치 된 것입니다.
     ```

  3. 프로젝트를 시작할 폴더에서 마우스 오른쪽 클릭 후 

  4. vscode 실행 => terminal에서 `npm install` 설치 

  5. 이후 Terminal에서 추가로 `npm i -g @vue/cli` 설치

     - 설치가 잘 되었나 `vue --version`으로 확인 (버전이 보이면 ok)

  6. _(선택사항)_ vscode의 추천 Extensions

     - `Vetur`

     -  `Vue VSCode Snippets`

     -  `Auto Rename Tag`

     -  `Auto Close Tag`

       ​	

- #### 🎉 프로젝트 생성

   ```bash
    $ vue create myproject		# myproject 프로젝트 생성
    $ cd myproject				# 생성한 myproject 폴더로 이동
    $ vue add router				# (선택사항) myproject 히스토리를 관리해줄 router기능 설치
    $ npm run serve				# myproject의 디폴트 서버 실행
   ```

  ```bash
    App running at:
      - Local:   http://localhost:8080/
      - Network: http://192.168.219.164:8080/
    
    이후 Local로 접속하면 생성된 서버 페이지 확인이 가능하다.
    또 같은 공유기를 사용하고 있는 기기에서 접속하여 서버 확인이 가능하다.
  ```

  ​	

- 생성된 프로젝트의 기본사항 파악

  1. Terminal창에 vue create myproject 명령어 입력 후

     < image src="/images/image-20200713230725043.png" caption=" " width="500px">

     이와 같은 창이 뜨는데 `default`로 설정하고 enter하고, 프로젝트를 시작한다.

     ​	

  2. 프로젝트 생성이 완료되면 `(router 미설치)`

     < image src="/images/image-20200713232004608.png" caption=" " width="200px">

     이와 같은 폴더 구조를 확인할 수 있다.

     ​	

  3. vue add router를 한다면. 두번의 묻는데 전부 y 로 처리`(router와 history mode 설정을 묻는 것)`

     < image src="/images/image-20200713231411715.png" caption=" " width="200px">



​	


### 여기서 잠깐✋ 

---

### 1. [VUE CLI 란?](https://simplevue.gitbook.io/intro/01.-vue-cli) (공식문서)

간단히 vue-cli 는 기본 vue 개발 환경을 설정해주는 도구

> **여기서 CLI 란 ?**
>
> **명령 줄 인터페이스(CLI, Command line interface) 또는 명령어 인터페이스**는 텍스트 터미널을 통해 사용자와 컴퓨터가 상호 작용하는 방식. 즉, 작업 명령은 사용자가 컴퓨터 키보드 등을 통해 문자열의 형태로 입력하며, 컴퓨터로부터의 출력 역시 문자열의 형태로 주어진다. (출처: 위키백과)

​	

### 2.  vue add router 필요한가?

< image src="/images/image-20200731224139667.png" caption=" " width="800px">

라우팅은 URI에 따라 해당하는 정적파일을 보여주는 방식. 이를 브라우저에서 구현 하는것이 **SPA** 개발의 핵심

>아이디어는 간단하다. 요청 URI에 따라 브라우저에서 DOM을 변경하는 방식. 대부분의 경우 도움이 되는 기능이기 때문에 상당이 추천한다. (추가로 navbar? 도 생기니 더욱 추천)
>
>**terminal**에 **vue add router** 를 입력하면 history 모드를 사용하겠나고 묻는데  **y** 를 입력해주면 된다. 

​	

### 3.  vue add router 후 History Mode 설정은 왜 필요한가?

< image src="/images/image-20200731224303419.png" caption=" " width="800px">

> vue add router를 설치하게 되면 `해쉬백 모드`와 `히스토리 모드` 중 디폴트로 `해쉬백 모드가 적용`된다.
>
> 만약 위의 그림에서 히스토리 모드를 사용하지 않더라도 나중에 URL에 있는 해쉬를 제거하기 위해 라우터의 **히스토리 모드**로 변경할 수 있다. 
>
> ```bash
> # src/router/index.js
> 
> const router = new VueRouter({
>   mode: 'history',						👈요거를 등록하면 history 모드 실행
>   routes: [...]
> })
> ```



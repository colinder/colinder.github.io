# Vue_setting


​	

# Vue

>Vue.js는 웹 애플리케이션의 사용자 인터페이스를 만들기 위해 사용하는 오픈 소스 프로그레시브 **자바스크립트 프레임워크**



## 1. 시작하기

> Vue VS Django
>
> 어느 정도 공부를 하면서 체득한 가장 큰 구분점은 "`Django는 App별로 응답을 구분`하고, `Vue는 Component별로 응답을 구분`한다."는 것이다.
>
> 큰 차이가 없어보이고, 큰 의미가 없어보일 수 있으나 이는 내가 vue를 이해하는데 가장 많은 도움이 된 부분이다.

---

- **개발 환경 설정 & 프로젝트 생성**

  1. [vscode](https://code.visualstudio.com/) 설치
  2. [nodeJS](https://nodejs.org/ko/) 설치 
  3. vscode 실행 후 terminal에서 `npm install` 설치
  4. vscode 실행 후 terminal에서 `npm i -g @vue/cli` 설치
     - 설치가 잘 되었나 `vue --version`으로 확인 (버전이 보이면 ok)
  5. _(선택사항)_ vscode의 Extensions에서`Vetur`와 `Vue VSCode Snippets` 설치
     - `Auto Rename Tag`, `Auto Close Tag`

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

     {{< image src="/images/image-20200713230725043.png" caption=" " width="500px">}}

     이와 같은 창이 뜨는데 `default`로 설정하고 enter하고, 프로젝트를 시작한다.

     ​	

  2. 프로젝트 생성이 완료되면 `(router 미설치)`

     {{< image src="/images/image-20200713232004608.png" caption=" " width="200px">}}

     이와 같은 폴더 구조를 확인할 수 있다.

     ​	

  3. vue add router를 설치한다면. 두번의 질문을 하는데 전부 y 로 처리`(router 설치)`

     {{< image src="/images/image-20200713231411715.png" caption=" " width="200px">}}



​	


### 여기서 잠깐✋ 

---

### 1. [VUE CLI 란?](https://simplevue.gitbook.io/intro/01.-vue-cli) (공식문서)

간단히 vue-cli 는 기본 vue 개발 환경을 설정해주는 도구

> **여기서 CLI 란 ?**
>
> **명령 줄 인터페이스(CLI, Command line interface) 또는 명령어 인터페이스**는 텍스트 터미널을 통해 사용자와 컴퓨터가 상호 작용하는 방식. 즉, 작업 명령은 사용자가 컴퓨터 키보드 등을 통해 문자열의 형태로 입력하며, 컴퓨터로부터의 출력 역시 문자열의 형태로 주어진다. (위키백과)



### 2.  vue add router 필요한가?

라우팅은 URI에 따라 해당하는 정적파일을 내려주는 방식이다. 이를 브라우져에서 구현해야 하는것이 **SPA** 개발의 핵심

>아이디어는 간단하다. 요청 URI에 따라 브라우져에서 돔을 변경하는 방식. 대부분의 경우 도움이 되는 기능이기 때문에 상당이 추천한다. (추가로 navbar? 도 생기니 더욱 추천)
>
>**terminal**에 **vue add router** 를 입력하면 history 모드를 사용하겠나고 묻는데  **y** 를 입력해주면 된다. 



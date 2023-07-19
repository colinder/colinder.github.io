# How To Deploy Django on Windows server Using Apache


​	

# How To Deploy Django on Windows server Using Apache

> Window에 backend(django)를 배포할 일이 생겨 방법을 정리하였다. (iis를 써보려했으나 실패했다.)

​	

<b>✨단계가 꽤 된다. </b>

### 요약

> 1. apache와 C++ 개발환경 세팅
>
> 2. django & <u>mod-wsgi</u> 설치 및 연동
> 3. Apache 서버 구동

​	

주의!

- 기본적으로 python은 설치되어있다고 가정

​	

​	

### step 00. 다운 받고 환경을 만들어 보자

1. apache

   - https://www.apachelounge.com/download/

     {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718171014500.png" width="1000px" >}}

     <span style="color: grey; float: right; font-size: 0.9em">작성일 기준 가장 최신버전으로 다운받는다.</span>

     ​	

   - 다운된 파일의 압축을 풀면 3개의 파일이 보인다.

     {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718171313119.png" width="1000px" >}}

     

     이중 <b>Apache24</b> 폴더를 통째로 잘라내고  C:\ 에 붙여넣었다.

     {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718171529660.png" width="1000px" >}}

     여기를 메인으로 사용할 것이다.

     ​	

     ​	

   - Apache 설치 파일을 옮겨왔으니 설치하자

     - 관리자 모드로 CMD를 켠다.

       {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718172208634.png" width="1000px" >}}

       ​	

     - `C:\Apache24\bin`로 이동하자.

       ```
       cd C:\Apache24\bin
       ```

       {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718172315520.png" width="1000px" >}}

       ​	

     - 이제 Apache 설치하자

       ```
       httpd.exe -k install
       ```

       뭔가 오류같은 내용들이 나오는데 출력된 내용 위에

       {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718172519238.png" width="1000px" >}}

       이것만 뜨면 일단 동작하는데 문제는 없는 것 같다. 

       ​	

     - 구동도 해보자

       ```bash
       httpd.exe -k start   
       ```

       아파치 서버 실행 명령어입니다. (중요)

       ​	

     - 이제 설치가 잘 되었는지, 잘 구동되는지 확인할 단계

       인터넷 주소창에 localhost라고 입력해보면

       <image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718172733840.png" width="1000px" style="border: 1px solid #cfcfcf">

       일하고 있다. (집, 공공장소인지 물어보는 경고창이 떠서 전부 체크하고 허용버튼 눌렀다.)

       ​	

     - 여기까지 확인했으면, 일단 서비스를 중지해두자.

       {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230719092636775.png" width="1000px" >}}

       ​	

     - Apache24를 찾아서 `중지`해둔다.

       {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230719092839262.png" width="1000px" >}}

       ​	

       ​	

       

2. Dev C++ 

   - https://visualstudio.microsoft.com/visual-cpp-build-tools/

     {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718173056957.png" width="1000px" >}}

     다운로드 후 실행되면, 

     ​	

   - C++ 이 써있는거 클릭 후 설치해준다. (빨간 박스만 클릭하고 설치하였다.)

     {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718173344257.png" width="1000px" >}}

​	

​	

​		

### step 01. django & <u>mod-wsgi</u> 설치 및 연동

잠깐. mod-wsgi는

> Apache에서 Python 기반 웹 응용 프로그램을 호스팅하기 위한 WSGI 호환 인터페이스를 제공하는 Graham Dumpleton의 Apache HTTP 서버 모듈.

​			

- 터미널 창을 켜고 

  - django 설치

    ```bash
    pip install django
    ```

    그리고 장고 프로젝트를 `C:\Apache24\htdocs`에 생성한다. (난 psgfront라는 이름으로 만들었다.)

    ```bash
    # django 프로젝트 생성 명령어
    django-admin startproject psgfront
    ```

    {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718175113073.png" width="1000px" >}}

    ✨ 혹시 정적파일들을 관리해야 한다면, 프로젝트 폴더로 들어가`(C:\Apache24\htdocs\psgfront)` static 이라는 폴더를 생성

    {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718175543845.png" width="1000px" >}} 

    ​	

    ​	

  - mod-wsgi 설치 & 설정값 확인

    ```bash
    pip install mod-wsgi
    ```

    설치 다 됬으면,

    ```bash
    mod_wsgi-express module-config  # 라고 입력 후 엔터
    ```

    이후 나오는 설정값(LoadFile, LoadModule, WSGIPythonHome 총 3개)은 등록해야 하는 값들이다. 

    ​	

- C:\Apache24\conf 로 이동해 httpd.conf 파일을 편집한다.

  {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718174603942.png" width="1000px" >}} 

  파일을 열고 맨아래로 가 아래 코드를 붙여넣는다. 

  ```bash
  # Django Project
  LoadFile "c:/python37/python37.dll"
  LoadModule wsgi_module "c:/python37/lib/site-packages/mod_wsgi/server/mod_wsgi.cp37-win_amd64.pyd"
  WSGIPythonHome "c:/python37"
  WSGIScriptAlias / "C:/Users/navar/Desktop/webproject/webproject/wsgi.py"
  WSGIPythonPath "C:/Users/navar/Desktop/webproject/"
  
  <Directory "C:/Users/navar/Desktop/webproject/webproject/">
      <Files wsgi.py>
          Require all granted
      </Files>
  </Directory>
  
  Alias /static "C:/Users/navar/Desktop/webproject/static/"
  <Directory "C:/Users/navar/Desktop/webproject/static/">
      Require all granted
  </Directory>
  ```

  ​	

- 여기서 아까  `mod_wsgi-express module-config`명령어로 확인한 설정값(LoadFile, LoadModule, WSGIPythonHome 총 3개)을 각각 변경한다.

- 나머지 값 확인

  - WSGIScriptAlias 

    - 생성한 django 프로젝트에 wsgi.py를 등록

      {{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230718175827731.png" width="1000px" >}} 

      저의 경우 `"C:/Apache24/htdocs/psgfront/psgfront/wsgi.py"`

      ​	

  - WSGIPythonPath

    - 생성한 django 프로젝트의 주소를 등록

    - 저의 경우 `"C:/Apache24/htdocs/psgfront/"`

      ​			

  - Directory

    - 생성한 django 프로젝트의 메인 app 주소를 등록

    - 저의 경우 `"C:/Apache24/htdocs/psgfront/psgfront/"`

      ​	

  - Alias /static & Directory

    - django 프로젝트에 생성한 static 폴더 주소를 등록

    - 저의 경우 `"C:/Apache24/htdocs/psgfront/static/"`

      ​	

      ​	

- 최종 결과

  ```bash
  LoadFile "C:/Users/kjong/AppData/Local/Programs/Python/Python310/python310.dll"
  LoadModule wsgi_module "C:/Users/kjong/AppData/Local/Programs/Python/Python310/lib/site-packages/mod_wsgi/server/mod_wsgi.cp310-win_amd64.pyd"
  WSGIPythonHome "C:/Users/kjong/AppData/Local/Programs/Python/Python310"
  WSGIScriptAlias / "C:/Apache24/htdocs/psgfront/psgfront/wsgi.py"
  WSGIPythonPath "C:/Apache24/htdocs/psgfront/"
  
  <Directory "C:/Apache24/htdocs/psgfront/psgfront/">
      <Files wsgi.py>
          Require all granted
      </Files>
  </Directory>
  
  Alias /static "C:/Apache24/htdocs/psgfront/static/"
  <Directory "C:/Apache24/htdocs/psgfront/static/">
      Require all granted
  </Directory>
  ```

  ​	

​		

​			

### step 02. 다시 서비스를 실행

아까 중지해두었던 서비스로 들어가 Apache2.4를 실행하면,

{{< image src="/images/How To Deploy Django on Windows server Using Apache.assets/image-20230719093512105.png" width="1000px" >}} 

잘 배포된 모습을 볼 수 있다. 

​	

​	

​	

​	

# 👀 마무리

> 배포는 잘 마무리 되었지만, 아직 설정할 것이 많이 있다. 차근히 찾아보고 진행했던 단계들도 다시 한 번 천천히 살펴보며 보강하는 것이 좋을 것 같다. 
>
> 명령어 모음 URL : https://github.com/colinder/How_To_Deploy_Django_on_Windows_server_Using_Apache/blob/main/README.md

​	

​	

​	

​	















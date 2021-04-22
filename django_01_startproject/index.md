# Django_01_start


​	

# Django 개발환경준비

- 편리함을 추구하기 위해 AWS Cloud9을 사용하여 진행

- https://ide.cs50.io/ 주소로 접속하면 진행이 가능하며, 진행을 위해선 Git 아이디가 있어야 한다.

  `Git`? : **버전 관리 시스템**이며 Git은 소프트웨어를 개발하는 기업의 핵심 자산인 소스코드를 효과적으로 관리할 수 있게 해주는 **무료**, **공개소프트웨어**. git에 대한 내용은 따로 정리해보도록 한다.

- 로그인까지 마치면 이제 Django 개발 준비 끝.

  <image src="/images/django_c9_main.png" caption="C9 접속 화면(블랙테마 적용되어 있음)">

  ​		

---

#### 👏 <span style="color:red">_21.04.18 기준 django : 3.2 V 으로 다시 정리 합니다._</span>

- 개발은 로컬(내 PC)진행합니다. 

  ### 1. DJango 설치

  ```python
  ## 개발을 진행하려는 폴더에서  
  > pip install django	# 설치가 진행될 것입니다.
  
  # 저는 DJ라는 폴더를 생성하고 진행하였습니다.
  ```

  ​			

  ### 2. 프로젝트 생성 & 구동 확인

  ```python
  ## 1. 프로젝트 생성
  # case 1
  > django-admin startproject blog		# blog라는 root폴더를 만들고 django프로젝트를 시작
  
  # case 2
  > django-admin startproject blog .	# . 을 붙이면, root폴더 생성없이 django프로젝트 시작
  
  #========================================================================================#
  ## 2. 프로젝트 구동 테스트
  > cd blog		# blog 폴더로 
  > python manage.py runserver 8080		# 프로젝트 구동 / 8080은 생략가능
  
  
  ✨Point
  1. blog라는 이름(root폴더)은 내가 원하는 프로젝트 명으로 변경이 가능
  2. cd blog → django의 명령어는 모두 manage.py가 존재하는 폴더안에서 실행해야 함, 이를 위한 이동
  ```

  ​		

  ### 3. 기본 설정(setting.py) 등록

  ```python
  # settings.py
  
  # 웹 사용자를 특정하는 부분
  ALLOWED_HOSTS = ['*']		# '*' == 모든 사용자가 입장가능
  
  # 프로젝트에 사용되는 APP 및 기능들을 등록
  INSTALLED_APPS = [
      'django.contrib.admin',
      'django.contrib.auth',
      'django.contrib.contenttypes',
      'django.contrib.sessions',
      'django.contrib.messages',
      'django.contrib.staticfiles',
  ]
  
  ...
  ...
  
  LANGUAGE_CODE = 'ko-KR'			#한글로 설정 변경
  
  TIME_ZONE = 'Asia/Seoul'		#시간대 서울로 변경
  ```

  <image src="/images/django_main.png" caption="여기까지 진행한 것을 보통 `로켓을 띄운다.`고 한다.">

  - 이와 같은 화면이 나오면 Basic Setting 완료입니다.

    ​		

    ​	

    ​	

  ## 더 알아보기

  1️⃣ *python manage.py runserver 8080 입력시 붉은 오류가 보일 수 있습니다.*

  <image src="/images/django_01_00.png" width="1000px">

  ​	

  2️⃣ *'python manage.py migrate'를 진행하라는 말 같은데 해당 내용은 나중에 'model'이라는 것을 설정할 때 자세히 보겠습니다. django의 공식문서 역시 지금 단계에서는 아래와 같이 안내합니다.*

  <image src="/images/django_01_01.png" width="1000px">

  ​	

  ​	



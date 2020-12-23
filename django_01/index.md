# Django_02_CRUD(01)


​	

# Django 개발환경준비

- 편리함을 추구하기 위해 AWS Cloud9을 사용하여 진행

- https://ide.cs50.io/ 주소로 접속하면 진행이 가능하며, 진행을 위해선 Git 아이디가 있어야 한다.

  `Git`? : **버전 관리 시스템**이며 Git은 소프트웨어를 개발하는 기업의 핵심 자산인 소스코드를 효과적으로 관리할 수 있게 해주는 **무료**, **공개소프트웨어**. git에 대한 내용은 따로 정리해보도록 한다.

- 로그인까지 마치면 이제 Django 개발 준비 끝.

{{< image src="/images/django_c9_main.png" caption="C9 접속 화면(`블랙테마 적용되어 있음`)">}}

​	

### 1. 프로젝트 시작하기

```python
django-admin startproject blog		# blog라는 django프로젝트를 시작
cd blog		# blog 폴더로 이동
python manage.py startapp articles		# articles라는 앱(기능폴더)을 생성

# Point🎈
# 1. blog라는 이름은 내가 원하는 프로젝트 명으로 변경이 가능
# 2. cd blog → django의 명령어는 모두 manage.py가 존재하는 폴더안에서 실행해야 함, 이를 위한 이동
```

- 구조를 만들었으니 구조들을 연결할 setting을 해주어야 한다. 




​	

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
    "articles",		# 등록
]

...
...

LANGUAGE_CODE = 'ko-KR'

TIME_ZONE = 'Asia/Seoul'
```

- 가장 기본이 되는 setting 완료



​	

```python
# 기본 설정이 잘 되었는지 확인할 서버구동
python manage.py runserver 8080		# 8080은 생략가능
```

​	

{{< image src="/images/django_main.png" caption="여기까지 진행한 것을 보통(`로켓을 띄운다.`)고 한다.">}}

- 이와 같은 화면이 나오면 Basic Setting 완료



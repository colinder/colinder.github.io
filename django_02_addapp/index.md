# Django_02_addapp


​		

# Django_app

> 이번에는 생성된 프로젝트에서 1. app을 추가해 등록해보고 2. 서버 접속시 첫 메인 페이지를 제작해봅시다.

​	

## 뜬금없이 CRUD를 알아보고 갑시다.

> **CRUD**는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말입니다. 사용자 인터페이스가 갖추어야 할 기능(정보의 참조/검색/갱신 등) 중 가장 기본이라고 생각되는 기술들입니다.

​	

### 이제 이어서 개발해봅시다.

​	

### 1. App 생성

지난번에는 프로젝트를 생성하고 setting.py에서 기본 설정만 변경해주었습니다. 이제 app을 생성해봅시다.

```python
## DJ폴더 안에서 django-admin startproject blog / django-admin startproject blog . 두가지 중
## 후자로 프로젝트를 생성하고 진행하겠습니다. 그러면 폴더 구조가 아래와 같습니다.
# DJ
#	blog
#	manage.py

> python manage.py startapp articles	# articles라는 app을 manage.py라는 python파일로 만들겠다.

# 입력 후 폴더 구조
# DJ
#	blog
#	manage.py
#	articles	👈👈

✨Point
1. 명령어는 manage.py가 있는 root폴더안에서 입력해야 합니다.
```

​	

### 2. 생성한 app을 프로젝트 폴더(blog)에 등록합니다.

```python
## blog/setting.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'articles',		#👈👈
]
```

 	

잠시 django의 개발 패턴을 보면

<image src="/images/django_02_03.png" width="1000px">

url로 요청을 접수한다는 것을 알 수 있습니다. urls.py로 들어오는 요청을 접수해봅시다.

​	

### 3. urls.py 등록

```python
## blog/urls.py

"""blog URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/3.2/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('articles.urls')),		#👈👈도메인 ''로 접속시 articles.urls로 요청을 보낸다.
]


	
✨Point 
1. 상단에 주석처리되어있는 내용을 읽어보며, 어떻게 url설정을 하는지 파악하는 것이 중요합니다.
```

이어서 articles 폴더에 urls.py 파일을 만들고 url을 만들어 줍시다.

```python
## articles/urls.py 생성

from django.urls import path
from . import views			#👈👈 articles 폴더에 있는 views.py폴더 사용할 수 있게 등록.

urlpatterns = [
    path('', views.main),	#👈👈도메인 ''로 접속시 views에 있는 main함수를 동작한다.
]


✨Point
1. blog/urls → articles/urls → articles/views 로 요청을 전달하는 과정을 이해하는 것이 중요합니다. 
```

​	

​	

이제는 views를 설정합니다.

<image src="/images/django_02_05.png" width="1000px">

### 3. articles/views.py 세팅

```python
## articles/views.py

from django.shortcuts import render

# Create your views here.
def main(request):
    return render(request, 'articles/main.html')	#👈👈 main함수는 render함수를 사용해 인자로 받은 request를 articles/main.html에 담아 사용자에게 보여준다.



# 아직 articles/main.html파일이 없습니다. 이제 우리가 사용자에게 응답할 페이지를 만들어야 합니다.
```

​	

​	

이제 마지막 templates를 설정합니다.

<image src="/images/django_02_06.png" width="1000px">

### 4. articles/templates/articles/main.html 생성

```python
## articles/templates/articles/main.html 생성

# !+tab 하면 html 기본 프레임이 나옵니다.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>main 페이지 입니다.</h1>		#👈👈
</body>
</html>
```

​	

​	

*완성된 화면은 아래와 같습니다.*

<image src="/images/django_02_02.png" width="1000px" style="border: 1px solid">

​		

​		

​		

# 👀요약

> 폴더 구조를 이해하면서 진행합시다!	

​	

​	

​	

## 더 알아보기

1️⃣ *urls.py를 통해 요청을 전달할때 왜? include를 사용했을까요? [django공식문서](https://docs.djangoproject.com/ko/3.2/intro/tutorial01/)에는 아래와 같이 설명합니다.*

<image src="/images/django_02_00.png" width="1000px">

​	

2️⃣ *왜 templates를 만드는데 articles폴더를 안에 하나 더 만들까요?*

 ∴ 이는 **1. templates 상속시 오류를 없게 하기 위함**과 __2. 대규모 프로젝트의 경우 하나의 app안에 다양한 templates 구성이 있는 경우 관리의 편리성__ 때문입니다. 

​		

 		

​	

​	

## 참고 했던 글

- https://senticoding.tistory.com/77

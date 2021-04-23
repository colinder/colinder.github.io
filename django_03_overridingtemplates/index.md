# Django_03_overriding Templates


​	

# Django Overriding Templates

> 웹개발을 하다보면 재활용되어야 하는 부분. 즉, <span style="color:red">변경되지 않아야 하는 부분</span>과, <span style="color:blue">변해야 하는 부분</span>이 있습니다. <span style="font-size:0.8em">(대표적으로 navbar)</span>  그리고 이런 개발의 편의성을 위해 Django는 Overriding 이라는 기술을 제공합니다.

​		

_지난 "app"에 이어 진행_

<span style="color:red">변경되지 않아야 하는 부분</span>은 **그림의 '배경'**이고, <span style="color:blue">변해야 하는 부분</span>은 **그림의 '디테일 요소'**라고 생각하면 이해가 쉽습니다. / 그럼 <span style="color:red">변경되지 않아야 하는 부분</span>을 먼저 제작해봅시다.

### 1. 변경 하지 않을 부분 == 배경 제작

```html
<!-- blog/templates/base.html 생성 -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1 style="background-color: green">여기는 base.html Overriding templates</h1>   👈
    {% block body %}					👈 디테일 요소 시작 위치
    {% endblock %}						👈 디테일 요소 종료 위치
</body>
</html>
```

​	

### 2. 배경을 setting.py에 등록

```python
## blog/setting.py

BASE_DIR = Path(__file__).resolve().parent.parent
...
...

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'blog' / 'templates'],		# 👈👈  '/'로 구분합니다.
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]


✨Point
1. TEMPLATES정보를 등록하는데, DIRS(디렉토리)가 BASE_DIR부터 'blog', 'templates' 주소 안에 있다.는 뜻.
```

​	

### 3. 변해야 하는 부분 == 디테일 요소 제작

```html
<!-- articles/templates/articles/overriding.html 생성 -->

{% extends 'base.html' %}		👈 setting.py에서 등록한 templates인 base.html을 불러오겠다. 

{% block body %}				👈 base.html의 디테일 요소 시작위치 
<div>
    <h1>Overriding 페이지입니다.</h1>
</div>
{% endblock %}					👈 base.html의 디테일 요소 종료위치 
```

_templates설정이 완료되었으니 이제 url을 연결해주어야 합니다._

​	

### 4. url 설정

도메인 root주소/overriding으로 접속하면 위에서 구현한 화면이 보이도록 만들어 보겠습니다.

```python
## articles/urls.py

from django.urls import path, include
from . import views

urlpatterns = [
    path('', views.main),
    path('overriding/', views.overriding),		# 👈
]
```

​		

### 5. views.py 설정

urls.py에서 views의 overriding함수를 실행한다고 구현했으니 overriding함수를 만들어 봅시다.

```python
## articles/views.py

from django.shortcuts import render

# Create your views here.
def main(request):
    return render(request, 'articles/main.html')

def overriding(request):									# 👈
    return render(request, 'articles/overriding.html')		# 👈 3.에서 만든 overriding.html을 노출
```

​	

​	

*완성된 화면은 아래와 같습니다.*

<image src="/images/django_03_00.png" width="1000px" style="border: 1px solid">

​	

​	

​	

# 👀요약

> 프로젝트를 진행하다 보면, 생각보다 <span style="color:red">변하지 않아야 하는 부분</span>과, <span style="color:blue">변해야 하는 부분</span>의 고려가 간단하진 않습니다. 하지만, **templates의 재사용성을 높이는 방식**으로 개발하는 것이 편리한 경우가 많이 있으니, 잘 알아보고 기획해서 사용하면 좋습니다.

​	

​	





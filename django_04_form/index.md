# Django_04_form


​	

# Django_form

> 이제 여러 html파일을 만들어 여러 화면을 돌아다닐 수 있게 되었습니다. 그렇다면 **A화면**에서 입력한 정보를 **B화면**으로 가져가고 싶다면 어떻게 해야 할까요?   __form__테그를 사용하면 됩니다. 

​	

### 👨‍💻 개발해봅시다.

기존에 만들었던 main.html을 활용해 만들어 보겠습니다.

### 1. form 태그 생성

```html
<!-- articles/templates/articles/main.html -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>main 페이지 입니다.</h1>
    <form action="/articles/output">					👈 action = form으로 입력된 정보를 보낼 곳입니다.
        제목 : <input type="text" name="title">		   👈 입력된 text타입 정보를 title이라는 이름으로 할당
        내용 : <input type="text" name="content">		   👈 입력된 text타입 정보를 content라는 이름으로 할당
        <button type="submit" > 제출 </button>		   👈 button을 누르면 form의 action주소로 이동
    </form>												👈
</body>
</html>
```

​		

### 2. urls.py 설정

위에서 articles/output으로 데이터를 보냈으니 

```python
## articles/urls.py

from django.urls import path, include
from . import views

urlpatterns = [
    path('', views.main),
    path('overriding/', views.overriding),
    path('articles/output/', views.output),		👈
]
```

​	

### 3. views.py 설정

위에서 articles/output/ 로 접속하면 views의 output함수를 동작시킨다고 선언했으니.

```python
## articles/views.py

from django.shortcuts import render

# Create your views here.
def main(request):
    return render(request, 'articles/main.html')

def overriding(request):
    return render(request, 'articles/overriding.html')

def output(request):											👈 output함수 선언
    title = request.GET.get('title')							👈 output함수의 request에서 title항목 추출	
    content = request.GET.get('content')						👈 output함수에 request에서 content항목 추출
    context = {													👈 추출한 두 요소를 context라는 하나로 묶고
        'title':title,											👈
        'content':content										👈
    }															👈
    return render(request, 'articles/output.html', context)		👈 render함수의 마지막 인자로 context 전달.
```

​	

### 4. output.html 제작

```html
<!-- articles/templates/articles/output.html -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>output 페이지 입니다.</h1>
    <h2>{{title}}</h2>		
    <h3>{{content}}</h3>
</body>
</html>
```

​	

*완성된 화면은 아래와 같습니다.*

- main.html

  <image src="/images/django_04_00.png" width="1000px" style="border: 1px solid">

- output.html

  <image src="/images/django_04_01.png" width="1000px" style="border: 1px solid">

  ​		

  ​		

  ​		

# 👀요약

> 데이터를 주고 난 후 url을 보면 **/?title="..."&content="..."**의 형태로 구성된 것을 볼 수 있습니다. 이는 앞으로 많이 보게 될 구성이니 익숙해지면 좋습니다.

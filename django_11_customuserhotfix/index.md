# Django_11_CustomUserhotfix


​			

# Django Foreign Key constraint failed

[ 주의! 본 포스팅은 내용이 정확하지 않을 수 있습니다. 수정할 부분은 피드백해주시면 감사드리겠습니다.]

문제는 Custom User Model 을 생성하면서 생긴 user_user 테이블과 기존에 `python manage.py createsuperuser` 명령어로 어드민 계정이 저장된 auth_user 테이블의 상속문제? 때문에 에러가 발생.

Custom User Model 을 사용할 때 settings.py 에 `AUTH_USER_MODEL = '내가만든app이름.User모델'` 을 추가하게 되는데 기존의 auth_user 테이블 대신 새로운 user_user 모델을 사용하겠다는 뜻.

그런데 나는 기존에 크롤러 데이터를 확인하기위해 auth_user 에 이미 admin 계정이 하나 생성되어있었던 상황. 거기에 user_user 가 하나 더 생겨버렸으니 유저 테이블을 혼동하기 시작한것이다. 그래서 django admin 에서 추가,수정이 안됐던것같다.

​			

#### 해결법

migrate 할때 Custom User Model 구현을 끝내고 하면 편하다.

1. db.sqlite3 파일 삭제(다른 DB를 사용한다면 그 DB초기화...)
2. Custom User Model 을 구현한 app 의 migrations 폴더 삭제
3. python manage.py makemigrations ( 만약 no detected 가 뜨면 python manage.py makemigrations 앱이름)
4. python manage.py migrate

이후에 어드민계정을 생성하고 django admin에 다시 들어가서 추가, 삭제를 해주면 에러가 나지 않을 것입니다.

​			

​	

​	

​	


# Django_05_Check DB connection


​	

# Django DB 연결 확인

​		

python manage.py createsuperuser 후.. default로 생기는 DB를 기준으로 실행

​		

```bash
## setting.py
AUTH_USER_MODEL = 'auth.User'   # default 설정.
```

​			

이후 terminal에서 

```bash
$ python manage.py shell_plus	# 모델에 접근하기 위해 shell_plus 실행

> Post = get_user_model()		# 생성된 모델의 user table 불러오기
> post = Post.objects.all()		# user table의 모든 값 가져오기

> for i in post:				# for문으로 user table값 순환
>     print(i)					# user table 값 출력
```

하면 연결된 모델의 값을 확인할 수 있다.


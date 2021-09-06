# Django_06_Django & Oracle18 error LOG


​		

# Django & Oracle 18 오류 일지

> 각종 스트레스의 원인에 대하여 해결방법을 정리합니다.

​					

1. ORA-00955: name is already used by an existing object

   > ```python
   > # 해결법
   > python manage.py makemigrations
   > python manage.py migrate --fake-initial
   > ```

   이미 migrate 한 내용을 수정하니 반영되지 않아 오류가 발생하였는데 위의 명령어는 migratie를 초기화해서 재설정 하는 것 같습니다. 

   ​				

2. 그리고 많은 경우에

   > ```python
   > python manage.py migrate --fake <appName> zero
   > python manage.py migrate <appName>
   > ```

   Django의 app별 migrate 초기화 명령 이게 많은 도움이 됩니다.

   ​			

   ​			

   ​			

# Javascript 이슈사항 정리

> 각종 javascript 스트레스 원인에 대하여 해결방법을 정리.

​				

1. object type에 dict 추가하기

   ```javascript
   foo['tracker'] = bar // from this...
   foo.tracker = bar;   // to this!!!!
   ```

   ​			

2. localstorage는 string 타입만 담을 수 있다. 하여 보통 응답으로 JSON 타입을 반환해주지만 이를 string으로 변환하여 localstorega에 담는데

   ```javascript
   이를 위해 
   JSON.stringify(담을 데이터) 의 형태로 저장한다.
   ```

   ​			

   ​			

   ​			

# Django 이슈사항 정리

> 각종 django 스트레스 원인에 대하여 해결방법을 정리

1. authenticate(조건) 와 model.objects.get(조건)의 출력값은 동일

   ```python
   A = authenticate(username=username, password=old_password)
   B = MyAuthUser.objects.get(username=username)
   
   print(type(A))		# <class 'accounts.models.MyAuthUser'>
   print(type(B))		# <class 'accounts.models.MyAuthUser'>
   
   A == B  # True
   ```

   ​			

2. `.catch(error => { console.log(error.response.data) }`  > catch는 이렇게 받으면 JsonResponse로 보낸 것을 확인할 수 있다.

   ​		

   ​		

   ​		

   ​	


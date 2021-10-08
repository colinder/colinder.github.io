# Django_10_CustomAuthUser_AbstractUser


​	

# Django <span style="color: #f2b141"><b>AbstractUser</b></span>

> 이전에 <span style="color: #4f8ae8">AbstractBaseUser</span>를 사용한 커스텀 사용자 인증 테이블을 구성해보았습니다. 해당 내용에 이어서 <span style="color: #f2b141"><b>AbstractUser</b></span>로 커스텀 사용자 인증 테이블을 구성해보고 createsuperuser 명령어 입력시, 관리자 계정이 커스텀한 테이블에 생성되게 해보겠습니다.

​	

## <span style="color: #f2b141"><b>AbstractUser</b></span>을 상속받아 커스텀을 진행

> 우선 <span style="color: #f2b141"><b>AbstractUser</b></span>는 Django에서 기본으로 제공해주는 사용자 인증 테이블의 요소를 그대로 가져와 사용하기 때문에 그 요소들은 어떤 것들이 있는지 알아보자.
>
> <b>id, password, last_login, is_superuser, username, first_name, last_name, email, is_staff, is_active, date_joined</b> 총 11개의 column이 존재하며 이외에 column을 추가 하고 싶을 때 코드를 추가로 작성합니다. 
>
> 전 이전에 <span style="color: #4f8ae8;"><b>AbstractBaseUser</b></span>의 경우와 같이, 추가로 **nickname**을 필수로 등록받는 것으로 시작하여 createsuperuser했을 때, myabuser 테이블에 nickname을 필수로 입력받는 관리자 계정이 생기도록 진행해보겠습니다.

​	

_아래 정리는 [이전에](https://colinder.github.io/django_08_abstractbaseuservsabstractuser/) <span style="color: #f2b141"><b>AbstractUser</b></span>관련 테스트를 진행한 후에 내용입니다. 즉, <b>프로젝트를 만들고, accounts라는 앱을 만들고, settings.py에 AUTH_USER_MODEL='accounts/myabuser'</b>를 등록한 후 진행합니다._

​	

#### 🤔 소스 코드를 작성해보려는데 궁금증이 생겼습니다. 

1. <span style="color: #f2b141"><b>AbstractUser</b></span>도 `BaseUserManager`가 필요한가?

   소스코드로 확인해보겠습니다.

   ```python
   # accounts/models.py
   
   from django.db import models
   from django.contrib.auth.models import AbstractUser
   
   # Create your models here.
   class MyAbUser(AbstractUser):
       nickname = models.CharField(max_length=100, default=None)
   ```

   까지만 설정하고 

   ```python
   # Terminal에 명령어 입력
   > python manage.py makemigrations
   > python manage.py migrate
   ```

   DB를 확인해보니

   <image src="/images/Django_10_CustomAuthUser_AbstractUser.assets\image-20211007133929980.png" width="400px">

   nickname column이 생겼고 이제,

   ```python
   # Terminal에 관리자 계정 생성을 시도
   > python manage.py createsuperuser
   ```

   했더니 제가 원했던 email, nickname, password를 받는 것이 아닌 default로 createsuperuser를 했을 때 입력받는 항목들이 나왔습니다.

   <image src="/images/Django_10_CustomAuthUser_AbstractUser.assets\image-20211007134406448.png" width="500px">

   즉, <span style="color: #4f8ae8;"><b>AbstractBaseUser</b></span>를 사용하든지 <span style="color: #f2b141"><b>AbstractUser</b></span>을 사용하든지 간에, 최소한 **커스텀된 관리자 계정을 생성**하기 위해서는 `BaseUserManager`가 필요하다는 것을 알았습니다.

   ​			

2. createsuperuser를 했을 때 email, nickname, password만 기록한 관리자 계정을 만들 수 있는가?

   우선 `BaseUserManager`를 추가한 소스코드

   ```python
   # accounts/models.py
   
   from django.db import models
   from django.contrib.auth.models import AbstractUser, BaseUserManager
   
   # Create your models here.
   class MyUserManager(BaseUserManager):
       def create_user(self, email, nickname, password=None):
           if not email:
               raise ValueError('Users must have an email address')
   
           user = self.model(
               email=self.normalize_email(email),
               nickname=nickname
           )
   
           user.set_password(password)
           user.save(using=self._db)
           return user
   
       def create_superuser(self, email, nickname, password):
           user = self.create_user(
               email,
               password=password,
               nickname=nickname,
           )
           user.is_admin = True
           user.save(using=self._db)
           return user
   
   class MyAbUser(AbstractUser):
       nickname = models.CharField(max_length=100, default=None)
   
       objects = MyUserManager()
   
       REQUIRED_FIELDS = ['nickname']
   ```

   모델에 변경사항이 있는 것은 아니니 **makemigrations & migrate**는 하지 않습니다. 이후에

   ```python
   # Terminal에 관리자 계정 생성을 시도
   > python manage.py createsuperuser
   ```

   하였지만, REQUIRED_FIELDS에 등록된 nickname을 추가로 받긴 하지만, 아직 email을 필수로 받지 않고, Username이 고윳값(PK)로 설정되어 있는 것 같습니다. 

   <image src="/images/Django_10_CustomAuthUser_AbstractUser.assets\image-20211007135527919.png" width="500px">

   ​	

   그렇다면, 고윳값(PK)를 변경할 수 있는 **USERNAME_FIELD**를 설정하고

   ```python
   # accounts/models.py
   
   class MyAbUser(AbstractUser):
       nickname = models.CharField(max_length=100, default=None)
   
       objects = MyUserManager()
       
   	USERNAME_FIELD = 'email'	#👈
       REQUIRED_FIELDS = ['nickname']
   ```

   createsuperuser를 해보겠습니다. 

   ```python
   # Terminal에 관리자 계정 생성을 시도
   > python manage.py createsuperuser
   ```

   하지만, 

   <image src="/images/Django_10_CustomAuthUser_AbstractUser.assets\image-20211007140106344.png" width="1000px">

   

   오류가 납니다. email을 unique하게 세팅해야 한다는 말 같아서 email의 필드설정을 해보겠습니다.
   
   ```python
   # accounts/models.py
   
   class MyAbUser(AbstractUser):
       email = models.EmailField(		   #👈
           verbose_name='email addresss', #👈
           max_length=255,
           unique=True,	               #👈
           default=None,
       )
       nickname = models.CharField(max_length=100, default=None)
   
       objects = MyUserManager()
       
   	USERNAME_FIELD = 'email'
       REQUIRED_FIELDS = ['nickname']
   ```

   이제 됩니다. 제가 작성한 코드가 동작하는지 확인하려 <b>일부로 오기재한 'email addresss'</b>으로 입력을 받고, 제가 원하던 모습인 email, nickname, password만 입력을 받으며 관리자 계정이 생성되었습니다.

   <image src="/images/Django_10_CustomAuthUser_AbstractUser.assets\image-20211007140531630.png" width="1000px">

   ​	

   DB를 확인해보니

   <image src="/images/Django_10_CustomAuthUser_AbstractUser.assets\image-20211007140842035.png" width="1000px">

   ​	

   <span style="color: #f2b141"><b>AbstractUser</b></span>를 상속받아 커스텀한 사용자 인증 항목 중, Default로 설정되는 값인 **is_superuser, is_staff, is_active, date_joinnd**를 제외한 email, nickname, password가 입력된 값으로 반영된 것을 확인하였습니다. 

   ​		

   아마, **is_superuser와 date_joined**는 필수값이 아니기 때문에 강제로 기록하고 싶지 않다면 방법이 있을 것입니다. 다만 이번에 알아보지는 않겠습니다.

   ​		

   이렇게 <span style="color: #f2b141"><b>AbstractUser</b></span>의 기본적인 사용법을 알아보았습니다. 

   ​		

   #### <span style="color: #4f8ae8;"><b>AbstractBaseUser</b></span>와 <span style="color: #f2b141"><b>AbstractUser</b></span>의 차이를 알아보았고 코드와 DB의 모습까지 살펴보았습니다. 이 둘의 차이와 장,단점을 이해하는데 많은 도움이 되었으면 합니다.

   ​		
   
   ​		
   
   ​		
   
   ​		


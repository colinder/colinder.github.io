# Django_09_CustomAuthUser_AbstractBaseUser


​	

# Django <span style="color: #4f8ae8">AbstractBaseUser</span>

> django는 기본적으로 username 과 password를 가지고 로그인합니다. 하지만 요즘에는 대부분의 웹사이트들이 email 과 password로 로그인(사용자를 인식 및 인증)을 합니다. 
>
> 제가 진행할 것은 django의 사용자 인증 테이블을 <span style="color: #4f8ae8">AbstractBaseUser</span>을 사용해 커스텀하고, createsuperuser 명령어 입력시, 관리자 계정이 커스텀한 테이블에 생성되게 하는 것입니다. 

​		

Django 공식문서에는 models.py에서 기본적인 **사용자**모델을 설정하고 이를 활용해 admin.py에서 **관리자**모델을 만드는 예제가 있어 이를 따라하며 약간의 수정을 더해 진행해보려 합니다.

​	

## 먼저 AbstractBaseUser를 상속받아 커스텀을 진행

>django의 예제에는 AbstractBaseUser라는 클래스를 상속받은 모델을 제작하는데, Abstract(추상적인)Base(기초)User(사용자)의 틀을 가져와 이를 활용해 내 입맛대로 커스텀 하는 과정.
>
>또한, 계정관련 모델을 만드는 경우에는 항상 두가지를 생각해야 합니다. **일반사용자**, **관리자**

`AbstractBaseUser`를 사용하면 로그인 방식도 변경할 수 있고<span style="font-size: 0.8em; color:grey">(내마음대로 구성)</span>, 원하는 필드들로 유저 모델을 구성할 수 있습니다. 아래는 `email`, `password`를 활용하여 로그인하고 추가적으로 `nickname`을 가지는 유저 모델을 구현해보겠습니다.

이 때 `BaseUserManager`를 상속하는 `UserManager`를 함께 정의하여 일반 유저 및 슈퍼유저의 생성 방식을 정의해줘야 합니다.  (또 `PermissionsMixin` 을 함께 상속하면 Django의 기본그룹, 허가권 관리 등을 사용할 수 있습니다. 하지만 여기서는 사용하지 않겠습니다.)

​		

## BaseUserManager를 상속받는 UserManager 정의

> BaseUserManager 클래스는 유저를 생성할 때 사용하는 헬퍼(helper) 클래스이며, 실제 모델(model)에 반영되는 것은 AbstractBaseUser을 상속받아 생성하는 클래스가 될 것입니다. 
>
> > 아래의 코드는 크게 두가지 class를 선언해 사용하고 있고, 동작의 흐름은,</br>아래 class(MyUser) → 위 class(MyUserManager) 순서로 동작합니다.

소스 코드

```python
# accounts/models.py

from django.db import models
from django.contrib.auth.models import AbstractBaseUser, BaseUserManager

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


class MyUser(AbstractBaseUser):
    email = models.EmailField(
        verbose_name='email addresss',
        max_length=255,
        unique=True,
    )
    nickname = models.CharField(max_length=100, default = None)

    is_active = models.BooleanField(default=True)
    is_admin = models.BooleanField(default=False)

    objects = MyUserManager()

    USERNAME_FIELD = 'email'

    REQUIRED_FIELDS = ['nickname',]

    @property
    def is_staff(self):
        return self.is_admin
```

**설명이 있는** 소스코드(위와 동일한 코드이나 자세한 설명이 있습니다.)

```python
# accounts/models.py

from django.db import models
from django.contrib.auth.models import AbstractBaseUser, BaseUserManager

# 위에서 설명한 일반 유저 및 슈퍼유저의 생성 방식을 정의할 BaseUserManager를 상속받는
# 나만의 사용자 매니저 모델을 생성합니다. 필수입니다.
class MyUserManager(BaseUserManager):
    def create_user(self, email, nickname, password=None):
		# 만약 email을 입력하지 않으면 바로 에러를 뱉습니다.
        if not email:
            raise ValueError('Users must have an email address')
		
        user = self.model(
            email=self.normalize_email(email),
            nickname=nickname
        )
		# 비밀번호는 암호화(set_password)함수를 사용해 변수에 저장합니다.
        user.set_password(password)
        # 등록된 사용자 정보를 DB에 저장합니다.
        user.save(using=self._db)
        # 마지막으로 사용자 정보를 return합니다.
        return user

    # python manage.py createsuperuser 명령어 입력시 = 관리자 계정 생성시
    def create_superuser(self, email, nickname, password):
		
        user = self.create_user(
            email,
            password=password,
            nickname=nickname,
        )
        user.is_admin = True
        user.save(using=self._db)
        return user


class MyUser(AbstractBaseUser):
    # DB에 저장할 column들을 설정합니다. 
	# 저는 email, nickname을 설정하겠습니다.
    # password는 필수값이기에 설정하지 않아도 입력받습니다.
    email = models.EmailField(
        verbose_name='email addresss',
        max_length=255,
        unique=True,
    )
    nickname = models.CharField(max_length=100, default = None)
    
    # 사용자 인증에 사용될 계정을 등록할 때 is_active와 is_admin도 필수값이기 때문에
    # 반드시 지정해주어야 합니다. (안하면 오류를 뱉습니다.)
    is_active = models.BooleanField(default=True)
    is_admin = models.BooleanField(default=False)
	
    # 위에서 생성한 일반 유저 및 슈퍼유저의 생성 방식을 정의할 나만의 사용자 매니저 모델을 가져옵니다.
    objects = MyUserManager()
	
    # unique한 값으로 설정할 것을 주로 USERNAME_FIELD로 설정합니다.
    # 저는 email의 중복을 허용하지 않고 이를 식별값(PK) 사용하겠습니다.(필수)
    USERNAME_FIELD = 'email'

    # createsuperuser 시 USERNAME_FIELD와 password 말고 추가로 받아야 하는 인자를 기재.
    # 상기 프로젝트의 경우 email, nickname, password를 받아야 하니
    # 기본으로 받는 인자인 USERNAME_FIELD = 'email'과 password 말고 나머지 요소를 기재
    REQUIRED_FIELDS = ['nickname',]

    # 이 아래는 선택사항입니다.
    def has_perm(self, perm, obj=None):
        "Does the user have a specific permission?"
        # Simplest possible answer: Yes, always
        return True

    def has_module_perms(self, app_label):
        "Does the user have permissions to view the app `app_label`?"
        # Simplest possible answer: Yes, always
        return True

    @property
    def is_staff(self):
        "Is the user a member of staff?"
        # Simplest possible answer: All admins are staff
        return self.is_admin
```

해당 사항을 정리하면서 `필수`라고 기재된 내용은 커스텀 사용자 인증 테이블을 만들 최소한의 필수 요소입니다.

​	

이후 MyUser를 settings.py에 등록.

```python
# setting.py 아무곳에나 ex) '앱이름.클래스명'
AUTH_USER_MODEL = 'accounts.MyUser'
...
```

​	

이제 Terminal에 관리자 생성 명령어 입력..

_잠깐! 제가 만든 코드들이 동작하는지 알기 위해서 **의도한 실수 표기법**인 "Email addresss"라는 input 메시지를 볼 수 있습니다._

<image src="/images/Django_09_CustomAuthUser_AbstractBaseUser.assets\image-20211007094210138.png" width="700px">

​	

이제 DB를 확인하는데 제가 만든 accounts앱의 myuser 테이블에 생성되었는지 확인합니다. 

<image src="/images/Django_09_CustomAuthUser_AbstractBaseUser.assets\image-20211007094318582.png" width="1000px">

또 django 기본 사용자 인증 테이블인 auth_user에 생기지 않았는지도 확인합니다.

<image src="/images/Django_09_CustomAuthUser_AbstractBaseUser.assets\image-20211007094426949.png" width="1000px">

의도한대로 관리자 계정이 생긴것을 확인하였고 <span style="color: #4f8ae8;"><b>AbstractBaseUser</b></span>의 기본적인 사용법을 알아보았습니다.

​	

#### 코드도 크게 복잡하지 않고 조금만 세세히 본다면 금방 이해할 수 있었고, 원하는 모습으로 설정하기에 쉬운 모습을 하고 있었습니다.

​	

​	

<span style="color: #f2b141"><b>AbstractUser</b></span>의 기본적인 사용법은 다음 글에 작성해보겠습니다.

​	

​		

​		

# **🔧오류 모음**

1. **Non-default argument follows default argument**

   ```python
   from django.contrib.auth.models import BaseUserManager
   
   class MyUserManager(BaseUserManager):
   	def create_user(self, email, password, username)  
       # 이렇게 작성했을 때 username에 오류가 발생하였다. 
   ```

   원인은 username을 인자로 받는데 email이나 password보다 앞서 입력을 받야아 한다 왜냐하면, default(value parameter는 생략)는 non-default 앞에 오면 안된다.는 규칙이 있기 때문.

   ​				

2. **Dependency on app with no migrations: accounts**

   > custom user의 모델을 만들고(models.py 수정 후) 마이그레이션 명령어를 입력하지 않아서 발생한 오류 같다.

   ```python
   > python manage.py makemigrations
   > python manage.py migrate
   # 명령어 입력 후 해결
   ```

   ​				

3. **You are trying to add a non-nullable field 'nickname' to myuser without a default; we can't do that (the database needs something to populate existing rows). Please select a fix:**
   
       1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
       2) Quit, and let me add a default in models.py
   
   > 이 오류는 DB, 모델을 수정할 경우에 자주 발생하는데, 기존에 makemigrations 를 통해 존재하는 정보들을 어떻게 수정할 지 정하지 않았기 때문.
   >
   > 코드상으로 해결하는 방법과 물리적으로 해결하는 방법이 존재.
   >
   > > 코드로는 __CharField(default = '')  or  CharField(null = True)__ 같이 default값과 null 을 허용해서 기존의 데이터들을 null 값이나 default 값으로 설정하는 방법과 <br>migrations 폴더안의 요소를 지우는 방법이 있으나 이는 기존의 데이터를 모두 지우고 새롭게 시작하는 방법이 존재(비추..)
   
   ​		
   
4. **django.db.migrations.exceptions.InconsistentMigrationHistory: Migration admin.0001_initial is applied before its dependency accounts.0001_initial on database 'default'.**

   


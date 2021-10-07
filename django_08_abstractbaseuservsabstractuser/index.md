# Django_08_AbstractBaseUser VS AbstractUser


​	

# Django <span style="color: #4f8ae8">AbstractBaseUser</span> VS <span style="color: #f2b141;">AbstractUser</span>

> Django로 프로젝트를 진행하면서 계정관련 내용을 등록하는 것을 가장 많이 했음에도, 아직 명확하게 차이를 알지 못하고 사용했던 것이 있었습니다. 
>
> 바로! User custom에 관한 부분이었는데요. 크게 <span style="color: #4f8ae8">AbstractBaseUser</span>와 <span style="color: #f2b141;">AbstractUser</span>를 사용하는 방법이 있는데 이 둘의 차이를 꼼꼼히 알아보기 위해 해당 포스팅이 작성되었습니다.

​	

## <span style="color: #4f8ae8">AbstractBaseUser</span> VS <span style="color: #f2b141;">AbstractUser</span>

> 프로젝트 생성 후 accounts 라는 app을 만들고 makemigrations & migrate를 하기 전

​	

### 1. Model 설정

<span style="color: #4f8ae8; font-size: 1.3em"><b>AbstractBaseUser</b></span>

_위치: accounts/models.py_

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006130519877.png" width="1000px">

<span style="color: #4ec983"><b>MyUser</b></span>라는 커스텀 table을 만들고 python manage.py runserver를 하게되면, 일단 오류없이 동작하는 것을 확인할 수 있습니다.

다만, 아직 변경사항을 DB에 반영하지 않았음으로 DB에 생성된 table의 모습은 확인할 수 없습니다. 

---

​		

<span style="color: #f2b141; font-size:1.3em"><b>AbstractUser</b></span>

_위치: accounts/models.py_

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006130424528.png" width="1000px">

<span style="color: #4ec983"><b>MyAbUser</b></span>라는 커스텀 table을 만들고 python manage.py runserver를 하게되면, 아래와 같은 오류를 만나게 됩니다. 

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006131313949.png" width="1000px">

해당 오류는 Django [공식문서](https://docs.djangoproject.com/en/2.2/topics/auth/customizing/#substituting-a-custom-user-model)에 해결방법이 있다. 

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006131605231.png" width="1000px" style="margin-top: 4px">

하여 settings.py 에 AUTH_USER_MODEL = 'accounts.MyAbUser'를 추가

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006140806022.png" width="1000px">

하였으나, 그럼에도

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006131758410.png" width="1000px">

의존성(Dependency) 오류가 발생한다. 이를 해결하기위해 검색해보니 이제 **makemigrations & migrate**를 진행해야 한다고 확인했습니다.

​	

초기 설정만 진행되었지만 일단, <span style="color: #4f8ae8;"><b>AbstractBaseUser</b></span>는 **makemigrations & migrate**없이 models.py에 등록만해도 오류없이 서버구동이 가능하나, <span style="color: #f2b141;"><b>AbstractUser</b></span>는 불가능하였습니다. 

​		

### 2. makemigrations & migrate 진행 후

```django
> python manage.py makemigrations
> python manage.py migrate
```

​	

<span style="color: #4f8ae8; font-size: 1.3em"><b>AbstractBaseUser</b></span>

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211007095955515.png" width="600px">

오류 없이 동작되었으며, DB에 아래와 같이 12개의 테이블이 생겼고, 커스텀한 accounts_myuser 테이블에 3개의 column이 기본으로 생기는 것을 확인했습니다.

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006133235477.png" width="470px">

---

​		

<span style="color: #f2b141; font-size:1.3em"><b>AbstractUser</b></span>

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211007100051412.png" width="600px">

우선 **makemigrations & migrate**을 진행하고 나니 python manage.py runserver가 오류 없이 동작되었습니다. 
DB를 확인해보면, 아래와 같이 커스텀한 accounts_myabuser테이블을 포함한 11개의 테이블이 생겼습니다. 

_생성된 column들은 django에서 default로 user를 생성하면 만들어지는 항목입니다._

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006133408681.png" width="470px">

​		

### 3. AbstractBaseUser VS AbstractUser 비교

생성된 DB모습의 차이는 아래와 같고 대응되는 table들을 연결해 보았습니다.

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006134144275.png" width="1000px">

간단히 <span style="color: #4f8ae8;"><b>AbstractBaseUser</b></span>를 사용하면 **auth_user**가 상대적으로 하나 더 생성된다는 차이가 있습니다. 그런데 그 안의 요소는 아래와 같습니다.

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006134324497.png" width="470px">

<span style="color: #f2b141;"><b>AbstractUser</b></span> 클래스를 상속받아 custom한 accounts_myabuser의 항복들과 동일했고, 이는 Django에서 기본으로 제공하는 인증에 사용되는 계정관련 테이블의 항목들입니다. 

​			

#### 즉, <span style="color: red">테이블 별 column 항목</span>을 기준으로 대응해보면

<image src="/images/Django_08_AbstractBaseUserVSAbstractUser.assets\image-20211006134734836.png" width="1000px">

이것이 맞는 표현이 됩니다.

> 간단히 <span style="color: #4f8ae8;"><b>AbstractBaseUser</b></span>를 사용하면,
> Django default DB Table 외에 사용자 인증에 사용할(커스텀할) 테이블(accounts_myuser)를 **추가로 만들**고,
>
> <span style="color: #f2b141"><b>AbstractUser</b></span>를 사용하면,
> django default DB Table 중 인증에 사용되는 user table 자체를 커스텀해 사용한다는 차이가 있다. 

​	

#### 🤔돌이켜 생각해보면 <span style="color: #f2b141;"><b>AbstractUser</b></span>의 경우 

**setting.py**에 <b>AUTH_USER_MODEL = 'accounts.MyAbUser'</b>를 추가해주는 행위가 이러한 차이를 유발하는 코드임을 알 수 있습니다. 이로인해 다른 설명들에 나와있는 표현인  "<span style="color: #f2b141;"><b>AbstractUser</b></span>는 무겁기<span style="font-size:0.8em; color: grey">(django의 default 사용자 인증 column들을 그대로 사용하기 때문)</span> 때문에 <span style="color: #4f8ae8;"><b>AbstractBaseUser</b></span>를 사용한다."가 이해됬으면 합니다.

​		

​	

​	

### 그.리.고!

내가 <span style="color: #4f8ae8;"><b>AbstractBaseUser</b></span>를 상속받아 커스텀하여 만든 table을 사용자 인증을 하는데 사용하고 싶다면, settings.py에서 AUTH_USER_MODEL = 'accounts_myuser'로 지정해줘야 합니다!

##### 결과적으로 인증에 사용할 테이블은 <span style="color: red">settings.py에 AUTH_USER_MODEL = "앱이름.테이블" 형태로 등록해주어 설정한다.</span> 는 점을 이해하면 좋겠습니다.

​	

​	

​	



# Understand_of_MariaDB


​	

# MariaDB

> mysql, mariaDB를 계속해서 사용하고 있지만, 계정과 데이터베이스 연결 관계 등 정확한 개념과 이해는 하지 않은 채 시간이 흘러가고 있어, 이번에 정리하고 기록합니다.
>
> <span style="font-size: .8em">전체적인 용어와 개념이 틀렸을 수도 있습니다. 제가 이해하기 쉬운 언어와 그림으로 정리하였습니다. 잘못된 부분이 있다면 꼭. 피드백을 부탁드립니다.</span>

​	

​	

## 진행과정

> "계정생성" > "database생성"> "권한부여" > "하이디에서 확인"


과정을 진행해보며 mariaDB의 구조와 흐름을 이해.

​	

​	

MariaDB를 설치한 후 MySQL Client에서 우선 진행.

<image src="/images/understand_of_MariaDB.assets\image-20221110164326372.png" width="1000px" style="display: block;">

​	

​		

​	

​	

## MariaDB 기본 정보

MariaDB를 설치하고 MySql Client로 접속을 시도하면

<image src="/images/understand_of_MariaDB.assets\image-20221111125510218.png" width="1000px" style="display: block;">

비밀번호를 입력하라는 메세지를 볼 수 있다. 

이는 프로그램을 설치할 때 root라는 계정의 비밀번호를 생성하는데 그떄 비밀번호를 입력하면 된다. 

<b>이 행위는 root라는 admin 계정으로 MariaDB 시스템에 접속하는 과정임을 알아야 한다. </b>

​	

비밀번호를 입력하면

<image src="/images/understand_of_MariaDB.assets\image-20221111125355174.png" width="1000px" style="display: block;">

mariaDB 시스템에 접속하게 된다.

​	

### Default database 확인

mariaDB는 설치하게 되면 default로 세팅이 되는데. 데이터베이스를 보자면,

```sql
show databases;
```

<image src="/images/understand_of_MariaDB.assets\image-20221110164817113.png" width="1000px" style="display: block;">

4개의 데이터 베이스가 생성되어 있다.

​	

​	

​	

### 여기서 잠깐💡

mariaDB는 '작업공간'이라는 개념이 있다.

<image src="/images/understand_of_MariaDB.assets\image-20221111093740029.png" width="1000px" style="display: block;">

현재는 mariaDB의 어떤 데이터베이스(작업공간)도 선택되지 않은 상태이다. 

​	

아래와 같은 명령어로 테이터베이스(작업공간)으로 접속할 수 있다. 

```sql
use mysql;     /* mysql이라는 데이터베이스(작업공간)을 사용하겠다.*/
```

<image src="/images/understand_of_MariaDB.assets\image-20221111093908545.png" width="1000px" style="display: block;">

잘 변경되었다면, MariaDB [<span style="color: red">mysql</span>] 과 같이 데이터베이스(작업공간)에 들어와 있는 것을 알 수 있다. 

​	

명령어를 입력하는데 내가 의도한 작업공간이 아닌 경우 에러가 날 수 있으니, <br><b>내가 의도한 작업공간안에 들어와서 명령어를 입력한다.</b> 는 개념을 가지고 있어야 한다. 

​		

​		

​	

​	

### Default 생성 계정 확인

또한 계정이 생성되는데 생성되는 계정은 <b>mysql 데이터베이스</b>안에 <b>user라는 테이블에 생성</b>된다. 

```sql
/* mysql의 user라는 곳에서 user와 host를 선택한다.*/
select user, host from mysql.user;         

/* 만약 mysql이라는 작업공간으로 접속해 명령어를 입력한다면, */
select user, host from user;
```

<image src="/images/understand_of_MariaDB.assets\image-20221110165054886.png" width="1000px" style="display: block;">

mariaDB라는 시스템에 등록된 계정이 여기에 쌓이게 되는 것이다.

​	

이외의 데이터베이스에는 user라는 테이블도 없다.

<image src="/images/understand_of_MariaDB.assets\image-20221110165203311.png" width="1000px" style="display: block;">

​	

참고로 mysql에 default로 생성된 테이블은 아래와 같다.

<image src="/images/understand_of_MariaDB.assets\image-20221110165611402.png" width="1000px" style="display: block;">

​	

<image src="/images/understand_of_MariaDB.assets\image-20221110170150110.png" width="1000px" style="display: block;">

information_schema는 총 79개의 테이블

​	

<image src="/images/understand_of_MariaDB.assets\image-20221110170315180.png" width="1000px" style="display: block;">

sys는 총 101개의 테이블

​	

<image src="/images/understand_of_MariaDB.assets\image-20221110170550097.png" width="1000px" style="display: block;">

performance_schema는 총 81개의 테이블이 있다.

​		

​		

즉, mariaDB라는 시스템을 설치하면, 기본적으로 4개의 데이터베이스 생성되고 

<image src="/images/understand_of_MariaDB.assets\image-20221110172248277.png" width="700px" style="display: block; margin:10px auto">

​		

​		

​	

시스템에 등록되는 계정은 <b>mysql이라는 데이터베이스</b>의 <b>user라는 테이블에서 관리</b>된다.

<image src="/images/understand_of_MariaDB.assets\image-20221110174713613.png" width="700px" style="display: block; margin:10px auto">

그리고 user 테이블에는 기본적으로 5개의 계정이 있고 이들이 어떤 역할을 하는지는 여기서 다루지는 않겠다. (실은 잘 모른다.)

​	

​	

​	

여기까지 MariaDB를 설치하면 생기는 기본적인 세팅에 대하여 살펴보았고,<br>지금부터 <b>"계정생성" > "database생성"> "권한부여" > "하이디에서 확인"</b>의 과정을 진행.

​	

​	

​	

## 계정 생성

이제 계정을 생성한다. 

<image src="/images/understand_of_MariaDB.assets\image-20221111092602836.png" width="1000px" style="display: block;">

처음 mariaDB에 접속하면 "none으로 어떤 데이터베이스도 선택되지 않았음"을 알 수 있다. 

​		

어떤 데이터베이스도 선택하지 않고 테스트 계정을 만들어 보면,

```sql
create user 'none_database_user'@'%' identified by '1234';  
/* 어디서든 접속할수있는(%) none_database_user라는 계정을 만들고 비밀번호는 1234 */
```

<image src="/images/understand_of_MariaDB.assets\image-20221111093143864.png" width="1000px" style="display: block;">

그 계정은 위에서 말한 mysql 데이터베이스에 user 테이블에 생성됨을 알 수 있다. 

​	

​	

​	

## 계정 삭제

```sql
delete from mysql.user where user='삭제할계정이름';	/* user테이블에서 삭제 */
delete from mysql.db where user='삭제할계정이름';		/* 계정DB에 반영 */
flush privileges;	/* 시스템 반영 */
```

<image src="/images/understand_of_MariaDB.assets\image-20221111094238203.png" width="1000px" style="display: block;">

위에서 설명했듯 **mysql 데이터베이스**에 **user라는 테이블**에 **계정이 생성**되는데,<br>계정을 삭제하는 위치(from)을 구체적으로 명시하지 않으면 정상적으로 동작 하지 않는다. 

> delete from <span style="color:red">user</span> where user='none_database_user';<br>
> delete from <span style="color:red">mysql.user</span> where user='none_database_user'; 
>
> *이 둘의 차이를 알아여하며, 내가 작업하고 있는 공간이 어디인지를 계속 생각해야 한다.*

​	

​	

​	

## 권한 부여

> 계정을 생성하면 기본적으로 information_schema 테이블이 생긴다. 
>
> **INFORMATION_SCHEMA란** MySQL 서버 내에 존재하는 DB의 메타 정보(테이블, 칼럼, 인덱스 등의 스키마 정보)를 모아둔 DB다. INFORMATION_SCHEMA 데이터베이스 내의 모든 테이블은 읽기 전용이며, 단순히 조회만 가능하다. 즉, 읽기전용(Read-only)으로 사용자가 직접 수정하거나 관여할 수는 없다.
>
> 진행해야 할 것은 <b>내가 사용할 데이터 베이스를 만들고 특정 계정에 그 데이터 베이스를 관리하라는 권한을 주어야 한다.</b> 

​	

<b>계정은 "human"</b>으로 만들고, <b>데이터베이스는 "box"</b>로 만들어보겠다.

```sql
/* 계정 생성 */
create user 'human'@'%' identified by '1234';

/* 데이터베이스 생성 */
create database box;
```

<image src="/images/understand_of_MariaDB.assets\image-20221111105536810.png" width="1000px" style="display: block;">

​	

- 계정 생성 확인

```sql
select user, host from mysql.user;
```

<image src="/images/understand_of_MariaDB.assets\image-20221111105724005.png" width="1000px" style="display: block;">

​	

- 데이터베이스 생성 확인

```sql
show databases;
```

<image src="/images/understand_of_MariaDB.assets\image-20221111105845749.png" width="1000px" style="display: block;">

​	

​	

생성한 human의 초기 권한을 확인해보면,

```sql
show grants for 'human'@'%';
```

<image src="/images/understand_of_MariaDB.assets\image-20221111111534592.png" width="1000px" style="display: block;">

무슨 말인지 모르겠습니다. 

​	

​	

**human 계정**에 **box 데이터베이스의 모든 권한을 주고**

```sql
grant all privileges on box.* to 'human'@'%';
/* 모든(all) 권한을 box데이터베이스하위 모든 곳(box.*)에 human계정에게 부여한다. */
```

<image src="/images/understand_of_MariaDB.assets\image-20221111111937918.png" width="1000px" style="display: block;">

​		

​	

이후 다시 human의 권한을 확인해보면,

<image src="/images/understand_of_MariaDB.assets\image-20221111112237534.png" width="1000px" style="display: block;">

> 나름의 해석
>
> 1. USAGE ON _*._* 의 의미는 모든 데이터베이스에 대하여 <b>접근</b>할 수 있다. 정도인 것 같습니다. 
> 2. box에 모든 권한을 받은 것을 확인

​	

​	

여기까지 진행한 사항을 DB에 반영하기 위해서는 

```sql
flush privileges;
```

<image src="/images/understand_of_MariaDB.assets\image-20221111122650811.png" width="1000px" style="display: block;">

를 입력

​	

​	

​	

​	

## 하이디(MariaDB GUI 시스템)으로 확인

> 하이디에서는 <b>계정</b> 별로 할당된 데이터베이스를 보는 시스템임을 알아 두어야 합니다.

​	

프로그램 시작 후 초기 모습

<image src="/images/understand_of_MariaDB.assets\image-20221111122837120.png" width="1000px" style="display: block;">

아무런 정보도 보이지 않음.

​	

​	

​	

**<span style="font-size: 1.6em">우선 root 계정에 대하여 확인해보겠습니다.</span>**

> 신규 > 세션이름 변경(root) > 사용자/암호 입력 > 열기

<image src="/images/understand_of_MariaDB.assets\image-20221111123054722.png" width="1000px" style="display: block;">

<span style="color:grey; font-size:.8em; display:flex; float:right">*세션 이름은 아무렇게나 설정해도 됩니다.</span>

​	

​	

​	

​	

위에서 만들었던 "box" 데이터베이스를 비롯, 기본으로 생기는 4개의 데이터베이스를 root계정으로 확인할 수 있음을 알 수 있다.

<image src="/images/understand_of_MariaDB.assets\image-20221111123119651.png" width="1000px" style="display: block;">

​		

​		

​	

​	

**<span style="font-size: 1.6em">human 계정으로 접근한다면?</span>**

> 신규 > 세션이름 변경(human) > 사용자/암호 입력 > 열기

<image src="/images/understand_of_MariaDB.assets\image-20221111123347245.png" width="1000px" style="display: block;">

<span style="color:grey; font-size:.8em; display:flex; float:right">*세션 이름은 아무렇게나 설정해도 됩니다.</span>

​	

​	

​	

​	

<image src="/images/understand_of_MariaDB.assets\image-20221111123441387.png" width="1000px" style="display: block;">

아까 권한을 준 **box 데이터베이스**와 default로 생성되는 **information_schema 데이터베이스만 접근이 가능**한 것을 알 수 있습니다. 

​		

​		

​		

​	

위의 상황을 이미지로 표현한다면, 

**<span style="font-size: 1.6em">root 계정으로 접속한다면?</span>**

<image src="/images/understand_of_MariaDB.assets\image-20221111131706389.png" width="1000px" style="display: block;">

​	

​		

​		

​		

**<span style="font-size: 1.6em">human 계정으로 접속한다면?</span>**

<image src="/images/understand_of_MariaDB.assets\image-20221111131803916.png" width="1000px" style="display: block;">

이와 같이 표현할 수 있습니다.

​	

​	

​	

## 간단히 정리하자면, 

1. 마리아디비에 등록된 계정마다 
2. 데이터베이스 (접근)권한을 부여하고 
3. 권한에 따라 접근할 수 있는 데이터베이스가 다르다.
4. 또한 데이터베이스마다 시스템<span style="color:grey; font-size: 0.8em">(ex. backend)</span>을 연결하여 개발을 한다.

​	

​	

계정과 데이터베이스의 관계 그리고 데이터베이스를 다른 시스템<span style="color:grey; font-size: 0.8em">(ex. backend)</span>에 연결하여서 사용한다고 했을 때,

- DB 계정과 backend에서 생서한 계정의 차이.

- DB 계정이 관리할 수 있는 내용와 한계 등을 이해하는데 도움이 되었으면 합니다.

​		

​	

​	


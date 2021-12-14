# MariaDB 외부접속 허용


​	

# MariaDB 외부 접속 허용

> 해당 MariaDB 공유는 **root PC에서 진행**하는 것이며, 외부에서 접속할 client는 별도로 설정할 건 없습니다.<br>
> 또한 무조건 이 방법이 맞다!라기보다는 제가 이렇게 하니 외부 접속이 되었다. 는 경험의 기록입니다.

​			

MySQL Client를 실행합니다. 

<image src="/images/MariaDB_share.assets/image-20211103112410570.png" width="300px" style="display: block;">

​	

실행하면 아래와 같은 초기 화면에 진입합니다.

<image src="/images/MariaDB_share.assets/image-20211103112506774.png" width="1000px" style="display: block; margin: 0 auto">

​		

MariaDB 설치를 진행하면서 등록한 password입력하면 아래와 같은 화면이 나옵니다. 

<image src="/images/MariaDB_share.assets/image-20211103112604983.png" width="1000px" style="display: block; margin: 0 auto">

​		

초기 접속의 경우, DB table이 'none'으로 설정되어 있는데, **공유를 원하는 table로 설정을 바꿔줍니다. **

```mariadb
> use 사용할DB
```

<image src="/images/MariaDB_share.assets/image-20211103112721876.png" width="1000px" style="display: block; margin: 0 auto">

​					

이제 접속을 허용할 PC 정보(ip)와 권한에 대하여 명령어를 입력합니다.

```mariadb
> grant all privileges on *.* to ‘아이디’@‘접속허용할PC의IP’ identified by ‘비밀번호’;
```

<image src="/images/MariaDB_share.assets/image-20211103130843442.png" width="1000px" style="display: block; margin: 0 auto">

### 이제 client에서 DB로 접속이 가능합니다. 😊

​		

마지막으로 설정한 내용이 잘 반영되어있는지 확인하기 위해 등록된 user계정을 확인해보겠습니다. 

```mariadb
> SELECT Host,User,plugin,authentication_string FROM mysql.user;
```

<image src="/images/MariaDB_share.assets/image-20211103132912196.png" width="1000px" style="display: block; margin: 0 auto">

이상으로 MariaDB 외부 접속 허용을 위한 설정을 완료하였습니다.

​	

​	

#### 생각보다 외부 접속 허용이 간단했습니다. 혹시 제가 문제가 있는 방법으로 진행한 것이라면 피드백 부탁드립니다!!

​	

​	

​	



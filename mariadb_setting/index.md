# MariaDB 설치 방법


​	

# MariaDB 설치 방법



## 1. 설치

https://mariadb.org/download/ 에서 자동으로 확인된 버전으로 다운로드 받습니다.

​		

<image src="/images/MariaDB_setting_00.png" width="500px" style="display: block; margin: 5px auto">

<center>Next 클릭</center>

​		

<image src="/images/MariaDB_setting_01.png" width="500px" style="display: block; margin: 5px auto">

<center>별도의 커스텀은 하지 않습니다.</center>

​			

<image src="/images/MariaDB_setting_02.png" width="500px" style="display: block; margin: 5px auto">

 'root 비밀번호 설정'과 'UTF 변환 및 외부에서 root 접속을 허용 할 것'인지 물어보는 데 이곳이 중요합니다.

단순 개발용이라면 상관없을 수 있으나 개인적으로 <span style="color: red">**실제 운용할 서버 or 개발 서버라도 외부에서 root에 접속하게 하는 것은 보안상 좋지 않을 것이라고 생각합니다.**</span>

<span style="color: red">**실제로 서비스를 운영 할 서버라면 절때 root를 원격지에서 사용할 수 없게 하는 것을 추천합니다.**</span>

root의 비밀번호를 등록하고, UTF-8 설정은 체크하겠습니다. <span style="font-size:0.8em">(나중에 변경할 수 있습니다.)</span>

<image src="/images/MariaDB_setting_03.png" width="500px" style="display: block; margin: 5px auto">

<center>이렇게 Next 하겠습니다.</center>

​					

<image src="/images/MariaDB_setting_04.png" width="500px" style="display: block; margin: 5px auto">

Database 서버에 접속할 **port**와 **서비스 이름**을 설정하는 곳입니다.<br>
*(혹시 Maria DB 설치전에 Mysql 서버가 설지되있는 경우에는 3306 port로 지정했을 때 충돌이 날 수 있으니 Maria DB의 port를 바꾸어사용하는 것을 추천합니다.)*

​					

<image src="/images/MariaDB_setting_05.png" width="500px" style="display: block; margin: 5px auto">

<center>Install 클릭하면 설치가 마무리됩니다.</center>

​		

​		

## 2. DB setting

<image src="/images/MariaDB_setting_06.png" width="100px" style="display: block; margin: 5px auto">

<center>정상적으로 설치가 되었다면 배경화면에서 해당 아이콘을 발견할 수 있습니다. (실행)</center>

​			

<image src="/images/MariaDB_setting_07.png" width="500px" style="display: block; margin: 5px auto">

<center>실행 초기 화면</center>

​			

<center>왼쪽 하단에 "<b><span style="color: green">⊕</span>신규</b>" 클릭 후 화면</center>

<image src="/images/MariaDB_setting_08.png" width="500px" style="display: block; margin: 5px auto">

​	

<image src="/images/MariaDB_setting_09.png" width="500px" style="display: block; margin: 5px auto">

<center>설치하면서 등록했던 root의 비밀번호를 입력 후 하단에 '열기'를 클릭합니다.</center>

​	

<image src="/images/MariaDB_setting_10.png" width="800px" style="display: block; margin: 10px auto">

<center>이제 새로운 DB table이 생성되었고 개발하는 프로그램과 연결하여 사용할 준비가 끝났습니다.</center>

​	

​	

​	



# pip freeze 시 @ 생기는 오류?

​		

# pip freeze 시 @ 생기는 오류?
​		


## 문제 상황

현재 환경에서 사용하고 있는 라이브러리 패키지 정보들을 가져올 때 보통 아래처럼 `pip freeze` 명령어를 사용해 `requirements.txt` 파일로 저장합니다.

```bash
pip freeze > requirements.txt
```

그럼 아래와 같이 해당 환경에 설치된 패키지들의 정보가 `requirements.txt` 파일에 저장됩니다.

```bash
<...>
clickhouse-driver==0.2.5
clickhouse-sqlalchemy==0.2.3
colorama @ file:///croot/colorama_1672386526460/work
colorlog==4.8.0
colour==0.1.5
commonmark @ file:///Users/ktietz/demo/mc3/conda-bld/commonmark_1630649545323/work
ConfigUpdater @ file:///croot/configupdater_1668698026863/work
confluent-kafka==2.1.1
connexion @ file:///opt/conda/conda-bld/connexion_1659800744294/work
cron-descriptor @ file:///opt/conda/conda-bld/cron-descriptor_1659858414281/work
croniter @ file:///croot/croniter_1666888073231/work
cryptography @ file:///croot/cryptography_1673298753778/work
debugpy==1.6.3
decorator==5.1.1
defusedxml==0.7.1
<...>
```

여기서 문제가 되는 부분은 해당 패키지 버전명이 `@ file://~~ `로 떠서 `requirements.txt` 을 이용해 해당 환경을 재구현할 수 없는 점은 문제로 판단했습니다.

​		

## 문제 원인

`pip` 패키지 19.1버전부터 버전명을 @ ~~로 명명하는 방법이 추가되었다고 한다.

이는 패키지가 아래처럼 특정 깃 레포 등에서 추가될 때 해당 정보를 저장하는데 유용하지만...

```bash
<package_name> @ git+https://githost/<repo>.git@<commit_id>
```

하지만 버전명이 file://<URL> 과 같이 로컬폴더로 지정된 경우는 해당 패키지 정보를  로컬환경이 아닌 이상 재구축하기 힘들었습니다.

​		

## 해결 방법

아래와 같이 format 옵션을 줘 해결할 수 있다.

```bash
pip list --format=freeze > requirements.txt
```

위 명령어로 `requirements.txt`를 만들면 아래와 같이 버전명이 정상적으로 출력됩니다.
```bash
<...>
clickhouse-driver==0.2.5
clickhouse-sqlalchemy==0.2.3
colorama==0.4.6
colorlog==4.8.0
colour==0.1.5
commonmark==0.9.1
ConfigUpdater==3.1.1
confluent-kafka==2.1.1
connexion==2.14.0
cron-descriptor==1.2.24
croniter==1.3.7
cryptography==38.0.4
debugpy==1.6.3
decorator==5.1.1
defusedxml==0.7.1
<...>
```

​		

​		

​		

​		

​		


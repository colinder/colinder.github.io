# docker compose option 모음

<br>

# 내가 자주 쓰는 docker compose option 모음
<br>

- `version`  → yaml 파일 포멧의 버전을 나타냄. 도커 컴포즈 버전마다 사용하는 yaml 포멧 버전이 있으므로 (도커 엔진 버전에 의존성 있음) 가능하면 최신 버전을 권장.

<br>

- `services` → 생성할 컨테이너들을 묶어 놓은 단위. 토커 컴포즈로 생성할 컨테이너 옵션을 정의. 이 항목에 쓰인 각 서비스는 컨테이너로 구현되며, 하니의 프로젝트로 도커 컴포즈에 의해 관리됨. 

  ex.)

  ```docker
  version: '3.7'

  services: 
    container_1:
      image: imFastApiImage
      container_name: myNameIsFastApi
      environment: 
        HOSTNAME: MASTER
      depends_on
        - mysql
      ports:
        - "9090:8080"
      volumes:
        - nfs:haha:/data
      ...
    
    mysql:
      image: seoch817/composetest:mysql
      

  volumes:
    nfs:haha:
      driver: local
      driver_opts:
        type: nfs
        o: ""
        device: ":/nfs/haha"
  ```

<br>

- `image` → `services` 의 컨테이너를 생성할 때 사용될 이미지를 설정. 이름의 형태는 *docker run과 같으며*, 만일 이미지가 도커에 존재하지 않으면, 도커 허브에서 자동으로 내려받음.

<br>

- `container_name` → 반드시 유니크 해야함. docker ps 검색시 보일 컨터이너의 이름.

<br>

- `environment` → docker run 명령어의 —env, -e 옵션의 내용을 미리 기재하는 곳. 서비스 컨테이너 내부에서 사용할 환경변수를 지정.

<br>

- `depends_on` → 컨테이너의 생성 순서와 실행 순서를 정의. 특정 컨테이너의 의존관계를 나타내며, 이 항목에 명시된 컨테이너가 먼저 생성되고 실행된다. 위 예제에서는 mysql 생성 후 container_1이 생성 됨.
비슷하게 `links` 라는 설정도 있는데 `depends_on` 은 서비스 이름으로만 접근할 수 있다는 것이 차이점.

<br>

- `ports` → docker run 명령어의 -p 옵션과 같이 서비스의 컨테이너를 개발할 포트를 설정. `HOST:CONTAINER` 포멧을 따르며, 지정도 가능.

  ```docker
  services:
    container_1:
      ports:
        - "9090:8080"
        #또는 아래의 형식으로도 작성 가능      
        - target: 8080       ## 컨테이너 내부 포트
          published: 9090    ## 호스트OS에서 공개할 포트
          protocol: tcp      ## 포트 프로토콜
          mode: host         ## 호스트 포트를 게시하기 위한 호스트 또는 로드 밸런싱할 스웜모드 포트를 입력
        
  ```

<br>

- `volumes` → 도커 컨테이너를 사용하여 서비스를 배포하다 보면 데이터를 저장해야 할 필요성이 있습니다. 이때 도커에서 제공하는 Volume을 사용하면 호스트 파일시스템과 컨테이너의 파일시스템을 마운트 하여 데이터를 관리하는 설정.

    - `o` → 마운트(컴퓨터 파일 시스템에서 다른 위치의 파일을 연결하는 프로세스) 설정.

      1. **nolock**: 이 옵션은 NFS(Network File System) 마운트에 적용됩니다. 기본적으로 NFS는 파일 락(locking) 메커니즘을 사용하여 동시에 여러 클라이언트가 파일에 접근하는 것을 제어합니다. 그러나 특정 시나리오에서 파일 락이 필요하지 않을 수 있습니다. 이 경우, **`nolock`** 옵션을 사용하여 파일 락을 비활성화할 수 있습니다.
      2. **soft**: 이 옵션은 NFS 마운트에서 사용됩니다. 기본적으로 NFS는 클라이언트와 서버 사이의 통신이 실패할 경우 계속해서 재시도합니다. 이는 네트워크 장애가 발생했을 때 일시적인 중단 없이 서비스를 유지하는 데 도움이 됩니다. 그러나 이런 재시도는 대기 시간이 길어질 수 있으므로 **`soft`** 옵션을 사용하여 타임아웃을 설정할 수 있습니다. 이는 클라이언트가 장애가 발생했을 때 오류를 반환하도록 하는 것입니다.
      3. **rw**: 이 옵션은 읽기/쓰기(read-write) 마운트를 나타냅니다. 즉, 마운트된 파일 시스템에 대해 읽기 및 쓰기 작업이 가능합니다. 기본적으로 대부분의 마운트는 읽기 및 쓰기가 가능하도록 설정됩니다.

      **`hard`** 옵션은 **`soft`**와 반대로 재시도를 포기하지 않고 계속해서 재시도하는 것을 의미합니다. 또한, **`intr`** 옵션은 마운트된 파일 시스템에 대해 중단 가능한 시스템 호출을 허용하도록 설정합니다.


<br><br><br><br>


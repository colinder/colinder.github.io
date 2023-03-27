# Command


​		

# 내가 자주 사용하는 docker 명령어 정리 

​	

​	

### 1. 현재 docker container 상태 확인하기

```dockerfile
docker ps -a
```

​	

​	

​	

### 2. 현재 docker image 조회

docker hub에서 받은 image와 내가 생성한 image를 보는 명령어

```dockerfile
docker images 
# 또는 docker image ls   /   둘은 같은 결과를 반환하는 명령어
```

​	

​	

​	

### 3. image 생성

1. hub에서 pull 받기

   ```dockerfile
   docker pull <다운받을 image 이름>
   ```

   - 다운받을 image 이름 & 종류는 Docker Hub Web(https://hub.docker.com/)에서 검색 후 존재를 확인하고 다운 받는다.

2. dockerfile 생성해서 구축하기

   dockerfile을 만들었다고 가정한 후 

   ```dockerfile
   docker build -t <내가 정하는image 이름> .
   # ex) docker build -t mydocker/nginx .
   ```

   ​	

   ​	

   ​				

### 4. image 삭제

<b>r</b>e<b>m</b>ove <b>i</b>mage

```dockerfile
docker rmi <image ID>
# ex) docker rmi 3e1b601d0fa6
```

​	

​	

​		

### 4-1. container 삭제하기 전에 image를 삭제하는 경우

- 강제로 삭제해야 하기 때문에 -f 옵션을 준다. <b>해당 경우 container도 삭제된다.</b>

  ```dockerfile
  docker rmi -f <image ID>
  # ex) docker rmi -f 3e1b601d0fa6
  ```

- none 으로 등록된 image 삭제 방법

  ```dockerfile
  docker rmi $(docker images -f "dangling=true" -q)
  # 또는 docker rmi -f $(docker images -f "dangling=true" -q)
  ```

  ​	

​			

​	

### 5. container 접속

```dockerfile
docker exec -it <컨테이너이름> /bin/bash
```

​	

​	

​	

### 6. container 실행

- 실행 > image를 만들고 해당 image로 컨테이너를 실행한다.(최초)

  ```dockerfile
  docker run <image ID>
  ```

- 재실행 > 멈춰 있는 container를 다시 실행한다.

  ```dockerfile
  docker start <containder NAME or containder ID>
  ```

​	

​	

​	

### 6-1. container 종료

```dockerfile
docker stop <containder NAME or containder ID>
```

​	

​	

​	

### 7. container 삭제

1. none으로 생긴 container 삭제 방법

   ```dockerfile
   docker rm $(docker ps --filter status=exited -q)
   ```

2. 일반 삭제 방법

   ```dockerfile
   docker rm <container ID> <...> <...>
   # ex) docker rm 3e1b601d0fa6 / 여러개인 경우 띄어쓰기로 구분 후 계속 입력
   ```

   ​	

   ​	

   ​	

### 8. container 이름 변경

- 실행중인 container의 이름을 변경하더라도 상태를 유지한다. <b>즉, 구동상태에 영향을 주지 않는다.</b>

  ```dockerfile
  docker rename <기존이름> <변경하고싶은이름>
  # ex) docker rename originalName changeName
  ```

  

​	

​	

​	

​	

​	

​	

​	


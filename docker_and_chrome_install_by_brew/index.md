# MacBookd에 Docker와 chrome 설치하기 by brew


​		

# Docker와 chrome 설치하기
맥북에 cli로 Docker와 chrome을 설치해보겠습니다. 
이전에 알아보았듯이 cli로 하는게 조금 더 깔끔 할 것 같아 이 방법으로요.

​		
​		
​		

## 그전에 잠시
`homebrew`를 써서 설치하려해서 이것을 먼저 설치 하겠습니다.
1. homebrew 설치
    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
​		

2. 설치 확인하기, 아래 명령어 입력해서 버전이 보이면 성공
   ```bash
   brew --version
   ```


​		
​		


​		
​		
​		
## docker와 google chrome 설치
1. 설치 명령어
    ```bash
    brew install --cask docker google-chrome
    ```
​		
    
2. 도커 설치 확인
    ```bash
    docker --version
    docker compose version
    ```
​		
    
3. 구글 크롬 설치 확인
   - 크롬 실행
​		
​		
​		

​		
​		
​		


​		
​		
​		

​		
​		
​		
이것도 매번 찾아보기 불편해서 한번 정리해보았습니다. 


​		
​		
​		

​		
​		
​		

​		
​		
​		

​		
​		
​		

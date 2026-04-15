# Macbook에 Miniconda 설치하기


​		

# Miniconda 설치하기
맥북에 cli로 miniconda를 설치해보겠습니다. 
다만, 인터넷에서 다운받는 거랑 무슨 차이가 있나 먼저 알아보겠습니다.

​		
​		
​		

### CLI vs .dmg 설치 비교
CLI(.sh 스크립트) 설치가 더 깔끔합니다. 이유는:
- dmg는 macOS 앱처럼 /Applications에 설치되거나 불필요한 GUI 관련 파일을 남길 수 있음
- .sh 스크립트는 지정한 디렉토리(기본 ~/miniconda3) 한 곳에만 모든 파일이 들어감
- 나중에 지울 때도 그 폴더 하나만 삭제하면 끝
- 개발자 워크플로우에 더 적합하고, 자동화/스크립트화도 쉬움


​		
​		
​		
### 설치 방법
1. 설치 스크립트 다운로드
    ```bash
    curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
    ```
    > Intel Mac이면 arm64 대신 x86_64 사용

<br/>

2. 설치 실행
    ```bash
    bash Miniconda3-latest-MacOSX-arm64.sh
    ```

    - 라이선스 동의: `yes`
    - 설치 경로: 기본값(`~/miniconda3`) 엔터 or 원하는 경로 입력
    - `conda init` 실행 여부: `yes` 권장 (셸에 자동으로 PATH 등록)
<br/>
<br/>
3. 설치 스크립트 삭제 (선택)
    ```bash 
    rm Miniconda3-latest-MacOSX-arm64.sh
    ```
<br/>

4. 터미널 재시작 후 확인
    ```bash
    conda --version
    ```


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
매번 찾아보기 불편해서 한번 정리해보았습니다. 


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

# Git_Mirroring


​	

# Git_Mirroring

>기존에 사용하고 있던 A_Repo에서 B_Repo로 커밋히스토리 그대로 복사가 필요할 때가 있습니다. 
>
>여기서 A_Repo를 그대로 가져온다는 의미는 단지 파일을 새롭게 만드는 것이 아니라 A_Repo에서 작업하던 commit 이력 모두를 그대로 이전하는 의미를 뜻합니다. 

​	

1. 터미널을 엽니다. 

2. 복사하고자 하는 저장소(A_Repo)의 bare clone을 생성합니다. 

   ```bash
   git clone --bare https://github.com/user/old-repository.git
   ```

3. 새로운 저장소(B_Repo)를 만들고 mirror-push를 진행합니다. 

   ```bash
   cd old-repository.git			👈 위에서 클론(bare)한 폴더로 이동
   git push --mirror https://github.com/user/new-repository.git
   ```

4. 2번 과정에서 클론된 저장소를 지웁니다.(선택사항)

   ​	

*대부분의 경우 위의 방법으로 미러링이 가능하지만, github 정책상 크기가 100MB를 넘어가는 파일이 단 한번이라도 commit되었다면, 오류가 발생하는 이슈가 있을 수도 있음.*

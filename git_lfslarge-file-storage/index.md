# Git_LFS


​	

# Git LFS(Large File Storage) 사용법

> Git은 **개당 파일에 100MB**의 용량 제한이 걸려 있다. (**전체 용량 제한은 없다.**) 다만, 프로젝트를 하다보면 대용량의 자료나 파일이 생기는 경우가 있다. 이때 용량 제한의 문제를 해소할 수 있는 방법이 LFS이다.
>
> LFS는 별도로 설치하여 사용하면 되며, 방법을 정리한다.



<image src="/images/git_LFS_00.png" width="1000px">

<p style='text-align:right; font-style:italic; color:grey'>*주소: https://git-lfs.github.com</p>

​	

1. [git LFS](https://git-lfs.github.com)을 다운을 받고 설치 합니다.

   ​		

2. git LFS를 적용할 폴더로 이동해 다음 명령어를 입력합니다.

   ```powershell
   $ git lfs install
   ```

   <image src="/images/git_LFS_01.png" width="1000px">

   ​	

3. git LFS로 관리할 파일을 지정해줍니다.

   ```powershell
   // mp4 확장자 모두를 lfs로 관리
   $ git lfs track "*.mp4"
           
   // 특정 파일만 lfs로 관리 (예시)
   $ git lfs track "video/hahaha.mp4"
   ```

   <image src="/images/git_LFS_02.png" width="1000px">

   ​	

4. 해당 경로의 설정을 저장할 .gitattributes 파일을 추가 합니다.

   ```powershell
   $ git add .gitattributes
   ```

   <image src="/images/git_LFS_03.png" width="1000px">

   ​	

5. 이후로는 평소 git을 사용하는 것처럼 commit / push하면 됩니다.

   ​		

   <p style="text-align:right; font-style:italic; font-size:1.4em"><b>끝</b></p>

   ​	

   ---

   ## 🙂부록

   *혹시 LFS를 풀고 싶다면 LFS설정한 폴더로 가서 아래 명령어를 입력하면 됩니다.

   ```powershell
   $ git lfs uninstall
   ```

   ​		

   *또 LFS없이 대용량 파일을 올리려고 하면 오류가 발생하는데, 잘 보면 LFS를 소개 해줍니다.

   <image src="/images/git_LFS_04.png" width="1000px">

   ​		

   ​	

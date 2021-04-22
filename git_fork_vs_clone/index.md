# Git_fork vs clone


​	

#	[Fork](https://git-scm.com/book/ko/v2/GitHub-GitHub-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-%EA%B8%B0%EC%97%AC%ED%95%98%EA%B8%B0)

>  fork는 다른 사람(프로젝트)의 github repository에서 내가 어떤 부분을 수정하거나, 기능을 추가 하고 싶을 때 해당 repository를 그대로 복제하는 기능. 

fork한 저장소는 원본 repository와 <span style='color:orange'><b>연결</b></span>되어 있습니다. 여기서 <span style='color:orange'>연결</span>되어 있다는 의미는, 원본 repository에 어떤 변화가 생기면 이는 forked된 나의 repository에도 반영될 수 있다는 것입니다. (단, [fetch](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EC%A0%80%EC%9E%A5%EC%86%8C)나 [rebase](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)의 과정이 필요합니다. fetch나 rebase를 하지 않았다면, 단순히 원본 repository를 복사해서 가져온 상태라고 할 수 있습니다.) 


graph LR;
    A[원본 repository_Ver.1.0.0] -->|fork| B[new repository_Ver.1.0.0]

---

graph LR;
    A[원본 repository에 변화 발생] --> |fetch or rebase 실행|B(new repository에 변화 반영)

---

graph LR;
    A[원본 repository에 변화 발생] --> |fetch or rebase 실행 X|B(new repository_Ver.1.0.0)


- fork는 보통 2가지 목적을 위해 사용합니다.
  1. 오픈소스 기여를 위해
  2. 기존 오픈 소스의 사본을 만들어서 새로운 버전을 만들어나가기 위해 (예를 들어 장고 오리지널이 망했는데 누군가 이 프로젝트를 fork해서 새로운 버전으로 이어나가는 거죠)

<p style='text-align:right; font-style:italic; color:grey'>*fetch: Git에서 어떤 브랜치의 코드를 받아오는 방법 중 하나. (또 다른 하나는 pull)</p>

<p style='text-align:right; font-style:italic; color:grey'>*rebase: Git에서 한 브랜치에서 다른 브랜치로 합치는 방법 중 하나. (또 다른 하나는 merge)</p>

​	

**🤔만약?**

내가 손본(개발한) 내용을 push 하면 나의 repository에만 변경사항이 저장되고 원본 repository에는 영향을 주지 못합니다. 다만, 원본 repository에도 나의 변경사항을 반영하고 싶다면, 원본 repository에 [pull request](https://wayhome25.github.io/git/2017/07/08/git-first-pull-request-story/)를 보내고 원본 repository 관리자가 수락하면 원본 repository에도 반영이 됩니다. == 타인 코드에 기여한다.

​		

# [Clone](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-Git-%EC%A0%80%EC%9E%A5%EC%86%8C-%EB%A7%8C%EB%93%A4%EA%B8%B0#_git_cloning)

> clone은 특정 repository를 내 local machine(ex. 내 노트북)에 복사하여 새로운 저장소를 만드는 기능. 

_clone하면_ 서버에 있는 프로젝트 히스토리를 포함한 `거의 모든` 데이터를 복사합니다. (`거의 모든`이라고 기록한 이유는 세부 명령어에 따라 clone되는 내용이 달라지기 때문입니다.)

또한 clone한 원본 repository의 remote가 origin으로 자동 설정됩니다. 만약 권한이 없다면 original repository의 로그를 보지 못하며, 해당 저장소로 push 하지도 못합니다.

​	

하지만!

로그까지도 모두 clone하는 등의 몇 가지 명령어가 존재합니다. 그중 많이 쓰이는 bare와 mirror에 대하여 정리해봤습니다.

1. **--bare**

   > [공식문서](https://git-scm.com/docs/git-clone)의 설명은 아래와 같음. 
   >
   > Make a *bare* Git repository. That is, instead of creating `<directory>` and placing the administrative files in `<directory>/.git`, make the `<directory>` itself the `$GIT_DIR`. This obviously implies the `--no-checkout` because there is nowhere to check out the working tree. Also the branch heads at the remote are copied directly to corresponding local branch heads, without mapping them to `refs/remotes/origin/`. When this option is used, neither remote-tracking branches nor the related configuration variables are created.

   간단히 bare옵션은 **HEAD**의 **refs** 정보가 clone됩니다.

   _*__HEAD__: 현재 작업중인 브랜치_

   _*__[refs](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EB%82%B4%EB%B6%80-Git-Refs)__: 알아보기 쉬운 이름으로 설정된 commit 이름을 “References” 또는 “Refs” 라고 부른다._

   ​		

2. **--mirror**

   > [공식문서](https://git-scm.com/docs/git-clone)의 설명은 아래와 같음.
   >
   > Set up a mirror of the source repository. This implies `--bare`. Compared to `--bare`, `--mirror` not only maps local branches of the source to local branches of the target, it maps all refs (including remote-tracking branches, notes etc.) and sets up a refspec configuration such that all these refs are overwritten by a `git remote update` in the target repository.

   간단히 mirror옵션은 **모든 브랜치**의 **refs** 정보가 clone됩니다.

   ​		

<image src="/images/git_bare_mirror.png" width="1000px">

​		

​	

```bash
✨Point

# fork와 clone의 차이
원본 저장소와 연결이 되어 있냐(fork) 아니냐(clone)?
```

​		

# *Push

Option

1. **--mirror**

   >Instead of naming each ref to push, specifies that all refs under `refs/` (which includes but is not limited to `refs/heads/`, `refs/remotes/`, and `refs/tags/`) be mirrored to the remote repository. Newly created local refs will be pushed to the remote end, locally updated refs will be force updated on the remote end, and deleted refs will be removed from the remote end. This is the default if the configuration option `remote.<remote>.mirror` is set.

   ​	
   
   ​	

**Special Thanks to Eric✨.**

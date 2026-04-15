# MacBook 기본 터미널 커스텀하기


​		

# 맥북 기본 터미널을 git bash 형태로 바꾸기

맥북 기본 터미널(zsh)을 사용하다 보면 기본 프롬프트가 밋밋하게 느껴질 때가 있습니다.  
이번 포스트에서는 git bash와 유사한 형태로 zsh 프롬프트를 커스텀하는 방법을 소개합니다.

> git과 miniconda가 설치된 상태에서 진행합니다.

​		
​		
​		

## 최종 결과물
- conda 가상환경 활성화 시 환경명 표시, 비활성화 시 미표시
- git으로 관리되는 폴더에서만 브랜치명 표시
- 사용자명은 보라색, 경로는 노란색, 브랜치는 하늘색

​		
​		
## 설정 방법
  1. `~/.zshrc` 열기   
      터미널을 실행하고 아래 명령어를 입력합니다.
      ```bash
      nano ~/.zshrc
      ```

​		

  2. 코드 추가

      편집기가 열리면 아래 코드를 빈 공간에 붙여넣습니다.

      ```bash
      setopt PROMPT_SUBST

      # conda 환경 표시 (기본색)
      conda_prompt() {
        if [[ -n "$CONDA_DEFAULT_ENV" ]]; then
          echo "%f($CONDA_DEFAULT_ENV) "
        fi
      }

      # git 브랜치 표시 (하늘색)
      git_prompt() {
        local branch
        branch=$(git rev-parse --abbrev-ref HEAD 2>/dev/null)
        if [[ -n "$branch" ]]; then
          echo "%F{cyan}($branch)%f "
        fi
      }

      PROMPT='$(conda_prompt)%F{magenta}%n%f %F{green}@%m%f %F{yellow}%~%f $(git_prompt)%F{green}$%f '
      ```

​		

  3. 저장 후 종료
   
      ctrl + O  →  Enter  →  ctrl + X

​		

  4. 변경사항 적용

      ```bash
      source ~/.zshrc
      ```

​		


​		

기본 프롬프트보다 현재 위치와 브랜치가 한눈에 들어와서 훨씬 편리합니다.


​		
​		
​		


​		
​		
​		


​		
​		
​		


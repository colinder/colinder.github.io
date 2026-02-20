# 내가 쓰는 vscode lint 설정


​	

## 방법 정리

1. vscode에서 확장프로그램 ruff를 설치 

2. user setting으로 가서

3. 아래 내용 입력

   ```python
   "[python]": {
       "editor.formatOnSave": true,
       "editor.codeActionsOnSave": {
           "source.organizeImports": "explicit"
       },
       "editor.defaultFormatter": "charliermarsh.ruff"
   },
   ```

   

​			

​	

​	

​	



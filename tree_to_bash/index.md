# bash에서 "tree"와 같이 폴더 구조 알고 싶을 때

<br>

# bash에서 하위 폴더 및 파일 구조 알고 싶을때 
아래 명령어를 입력하면 얼추 필요없는거 제외하고 보입니다. 



```bash
find . -name '__pycache__' -prune -o -name '.vscode' -prune -o -name '.git' -prune -o -name '*.pyc' -prune -o -print | sed -e 's/[^\/]*\//|   /g' -e 's/|   \([^|]\)/+--- \1/'
```
<br><br><br><br>

# 내가 쓰는 vscode lint 설정


​	

## 방법 정리

1. vscode에서 확장프로그램 ruff, ESLint, Prettier - Code formatter, Stylelint, DotENT 설치

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
    "[javascript]": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },    
    "[javascriptreact]": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[css]": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode",
        "editor.codeActionsOnSave": {
            "source.fixAll.stylelint": "explicit"
        }
    },
    "files.exclude": {
        "**/.git": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/.DS_Store": true,
        "**/Thumbs.db": true,
        "**/__pycache__" : true,
        ".vscdoe": true
    },
    "editor.fontSize": 14,
    "editor.fontFamily": "Cascadia Code, Menlo, Monaco, 'Courier New', monospace",
    "editor.mouseWheelZoom": true,
    "python.createEnvironment.trigger": "off",
    "workbench.colorTheme": "Dark Modern",
    "terminal.integrated.fontFamily": "Cascadia Code",
    "terminal.integrated.fontSize": 14,
    "security.workspace.trust.untrustedFiles": "open"
   ```

   

​			

​	

​	

​	



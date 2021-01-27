# BEAKJOON 4949, 10828, 1874, 17298


​	

### 4949_균형잡힌 세상

```python
import sys

while True:
    text = sys.stdin.readline().rstrip()
    if text == ".":
        break
    stack =[]
    result = "yes"
    for i in text:
        if i.isalpha() or i == " ":
            continue
        elif i == "(" or i == "[":
            stack.append(i)
        elif i == ")":
            if not stack or stack[-1] != "(":
                result = "no"
                break
            else:
                stack.pop()
        elif i == "]":
            if not stack or stack[-1] != "[":
                result = "no"
                break
            else:
                stack.pop()

    if not stack:
        print(result)
    else:
        print('no')
```

​		

### 10828_스택

```python
import sys

stack = []
commands = []
for i in range(int(sys.stdin.readline().rstrip())):
    commands.append(sys.stdin.readline().rstrip().split())
    
for command in commands:
    if command[0] == "push":
        stack.append(int(command[1]))
    elif command[0] == "pop":
        if stack:
            print(stack.pop())
        else:
            print(-1)
    elif command[0] == "size":
        print(len(stack))
    elif command[0] == "empty":
        if stack:
            print(0)
        else:
            print(1)
    elif command[0] == "top":
        if stack:
            print(stack[len(stack)-1])
        else:
            print(-1)
```

​		

### 1874_스택 수열

```python
import sys


N = int(sys.stdin.readline().rstrip())
Numbers = [i for i in range(1, N+1)]

Sequence = []
for i in range(N):
    v = int(sys.stdin.readline().rstrip())
    Sequence.append(v)

result = ""
stack = []
ans = []
for i in range(N):
    if stack and stack[-1] == Sequence[i]:
        ans.append(stack.pop())
        result += "-"
    elif not Numbers:
        while stack:
            ans.append(stack.pop())
        break
    elif Numbers[0] <= Sequence[i]:
        try:
            while Numbers[0] <= Sequence[i]:
                stack.append(Numbers.pop(0))
                result += "+"
            ans.append(stack.pop())
            result += "-"
        except IndexError:
            if stack[-1] == Sequence[i]:
                ans.append(stack.pop())
                result += "-"
                continue
    
if ans == Sequence:
    for i in result:
        print(i)
else:
    print("NO")

# 뭔가 좀 지저분해보이지만..
# 입력받은 수열의 요소 마다 검증을 하고 싶었고
# 각 요소마다 진행사항을 등록하고 싶었다. 
# 예를 들어) 수열의 첫 번재 '4'가 나오기 위해선. 
# 1,2,3,4가 스택에 들어갔다가 마지막 4가 출력되면 된다.
# 이런 과정을 코딩하고 싶었다. 

# 인터넷에 있는 까리한 코딩과 비교해봤는데.
# 다행이 내 코드가 속도면에서 우위를 점했다.
# 메모리는 졌음. ㅎ
```

​		

### 17298_오큰수

```python

```

​		

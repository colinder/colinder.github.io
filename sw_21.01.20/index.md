# SW Expert Academy_D4 1218, 1219, 1222, 1223


​	

### D4_1218_괄호 짝짓기

```python
def Pair_bracket():
    for i in range(L):
        if q[i] in check:
            ni = i - 1
            while True:
                if q[ni] == 0:
                    ni -= 1
                else:
                    break
            a = q[ni]
            b = check[q[i]]
            if a == b:
                q[i] = 0
                q[ni] = 0
            else:
                return 0
    return 1

check = {")" :"(", "]":"[", "}":"{", ">":"<"}
for T in range(10):
    L = int(input())
    q = list(input())

    print(f'#{T+1} {Pair_bracket()}')
```

​	

### D4_1219_길찾기

```python
from collections import deque

def BFS():
    visited[0] = 1
    while q:
        a = q.popleft()
        if a == 99:
            return 1
        if visited[a] == 0:
            for i in range(len(arr[a])):
                q.append(arr[a][i])
            visited[a] = 1
    return 0   

for _ in range(10):
    T, N = map(int, input().split())
    arr = [[] for _ in range(100)]
    route = list(map(int, input().split()))
    visited = [0 for _ in range(100)] 

    for i in range(0, N*2, 2):
        arr[route[i]].append(route[i+1])
        
    q = deque()
    for i in range(len(arr[0])):
        q.append(arr[0][i])

    print(f'#{T} {BFS()}')

# 빈 2차원 리스트 만드는 법
# [[] * 100]  은 안됨
# [[] for _ in range(N)] 으로 생성
```

​	

### D4_1222_계산기1

```python
for T in range(10):
    L = int(input())
    text = input()

    # 후위 표기법 알고리즘 (BEAKJOON - 1918을 먼저 풀어보세요.)
    operator = {"+":1, "-":1, "*":2 , "/":2, "(":0, ")":0}
    stack = []
    result = ""

    for i in text:
        if i not in operator:
            result += i
        elif i == "(":
            stack.append(i)

        elif i == ")":
            while stack and stack[-1] != "(":
                result += stack.pop()
            stack.pop()
        else:
            while stack and operator[i] <= operator[stack[-1]]:
                result += stack.pop()
            stack.append(i)

    while stack:
        result += stack.pop()


    # 문제 풀이.
    total = 0
    for i in text:
        if i == "+":
            continue
        total += int(i)

    print(f'#{T+1} {total}')
```

​	

### D4_1223_계산기2

```python
for T in range(10):
    L = int(input())
    text = input()

    # 후위 표기법 알고리즘 (BEAKJOON - 1918을 먼저 풀어보세요.)
    operator = {"+":1, "-":1, "*":2 , "/":2, "(":0, ")":0}
    stack = []
    result = ""

    for i in text:
        if i not in operator:
            result += i
        elif i == "(":
            stack.append(i)

        elif i == ")":
            while stack and stack[-1] != "(":
                result += stack.pop()
            stack.pop()
        else:
            while stack and operator[i] <= operator[stack[-1]]:
                result += stack.pop()
            stack.append(i)

    while stack:
        result += stack.pop()

    # 문제 풀이.
    aws = []
    idx = []
    for i in range(L):
        if i in idx:
            continue
        if text[i] in "0123456789":
            aws.append(int(text[i]))
        elif text[i] == "*":
            aws[-1] = aws[-1]*int(text[i+1])
            idx.append(i+1)
    print(f'#{T+1} {sum(aws)}')
```

​	

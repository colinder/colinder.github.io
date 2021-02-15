# SW Expert Academy_D4 1258, 1249, 1238, 4261


​	

### D4_1258_행렬찾기

```python
def check(x, y):
    dx, dy = 0, 0
    while x + dx < N and arr[x+dx][y]:
        dx += 1
    while y + dy < N and arr[x][y+dy]:
        dy += 1
    result.append([dx*dy, dx, dy])
            
    for i in range(x, x+dx):
        for j in range(y, y+dy):
            arr[i][j] = 0

for T in range(int(input())):
    N = int(input())
    arr = [list(map(int, input().split())) for _ in range(N)]

    result = []
    for i in range(N):
        for j in range(N):
            if arr[i][j] != 0:
                check(i, j)
    
    aws = sorted(result, key=lambda x: [x[0], x[1]])
    print(f'#{T+1} {len(aws)}', *[str(i[1]) + ' ' + str(i[2]) for i in aws])

    
# DFS로 구현해보려 했는데.
# 내가 원하는 위치값을 반환하고 바로 종료시키는 것이
# 불가능해서. 최소한의 값인 가로 세로 이동값만을 뽑아
# 정리하였다.
# 마지막에 정렬이 더 복잡했다..
```

​	

### D4_1249_보급로

```python
from collections import deque

dx = [0,1,0,-1]
dy = [1,0,-1,0]

for T in range(int(input())):
    N = int(input())
    arr = [list(map(int, input())) for _ in range(N)]
    
    visited = [[float('inf') for _ in range(N)] for _ in range(N)]
    visited[0][0] = arr[0][0]
        
    q = deque([[0, 0]])
    while q:
        x, y = q.popleft()
        for i in range(4):
            cx = x + dx[i]
            cy = y + dy[i]
            if 0 <= cx < N and 0 <= cy < N:
                Sum = visited[x][y] + arr[cx][cy]
                if visited[cx][cy] > Sum:
                    visited[cx][cy] = Sum
                    q.append([cx, cy])

    print(f'#{T+1} {visited[N-1][N-1]}')
    
    
# DFS로 구현해보려 했지만, 특정 위치에 왔을 때 종료를 
# 구현하는데 어려움이 있어, BFS로 바꾸어 구현.
```

​	

### D4_1238_Contact

```python
from collections import deque

for T in range(10):
    N, S = map(int, input().split())
    node = list(map(int, input().split()))

    arr = [[] for _ in range(N)]
    for i in range(0, N, 2):
        arr[node[i]].append(node[i+1])
        
    visited = [0] * (N+1)
    
    visited[S] = 1
    q = deque([S])

    while q:
        v = q.popleft()
        step = visited[v]
        for i in arr[v]:
            if visited[i] == 0:
                q.append(i)
                visited[i] = step + 1
    
    for j in range(N+1):
        if visited[j] == step:
            result = j
    
    print(f'#{T+1} {result}')
```

​		

### D4_4261_빠른 휴대전화 키패드

```python
anycall = {'2':"abc", '3': "def", '4':"ghi", '5':"jkl", '6':"mno", '7':"pqrs", '8':"tuv", '9':"wxyz"}

for T in range(int(input())):
    S, N = map(int, input().split())
    words = list(input().split())

    result = 0
    for x in range(len(words)):
        count = 0
        for i, j in enumerate(str(S)):
            a = words[x][i]
            b = anycall[j]
            if words[x][i] in anycall[j]:
                count += 1
            else:
                break
        if count == len(str(S)):
            result += 1
        else:
            count = 0

    print(f'#{T+1} {result}') 

            
# 비교적 쉬운 문제.
```

​	

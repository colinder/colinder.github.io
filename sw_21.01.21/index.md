# SW Expert Academy_D4 1226, 1227, 1231, 


​	

### D4_1226_미로1

```python
dx = [0,1, 0, -1]
dy = [1,0, -1, 0 ]

def IsSafe(x, y):
    if 0<= x < 16 and 0 <= y < 16:
        return True

def DFS(x, y):
    global result
    visited[x][y] = 1
    if arr[x][y] == 3:
        result = 1
    for i in range(4):
        nx = x + dx[i]
        ny = y + dy[i]
        if IsSafe(nx, ny) and visited[nx][ny] == 0 and arr[nx][ny] != 1:
            DFS(nx, ny)
            

def Find_Start():
    for i in range(16):
        for j in range(16):
            if arr[i][j] == 2:
                return i, j


for T in range(10):
    t = input()
    arr = [list(map(int, input())) for _ in range(16)]
    visited = [[0 for _ in range(16)] for _ in range(16)]

    x, y = Find_Start()
    result = 0
    DFS(x, y)
    
    print(f'#{T+1} {result}')


# 재귀를 사용할 시 return은 불편한 점이 많다.
# 하여 특정 조건에서의 값이 필요한 경우.
# 변수를 설정해서 그 값을 바꾸는 편이 간편하다. 
# 다만 런타임에 조심해야 한다.
```

​	

### D4_1227_미로2

```python
from collections import deque

dx = [0, 1, 0, -1]
dy = [1, 0, -1, 0]

def IsSafe(x, y):
    if 0<= x < 100 and 0 <= y < 100:
        return True

for T in range(10):
    t = input()
    arr = [list(map(int, input())) for _ in range(100)]
    visited = [[0 for _ in range(100)] for _ in range(100)]

    result = 0
    q = deque([[1, 1]])
    while q:
        x, y = q.popleft()
        visited[x][y] = 1
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            if arr[nx][ny] == 3:
                result = 1
                break
            if IsSafe(nx, ny) and visited[nx][ny] == 0 and arr[nx][ny] == 0:
                q.append([nx, ny])
        if result:
            break

    print(f'#{T+1} {result}')
```

​	

### D4_1231_중위순회

```python
def DFS(i):
    global result
    if i <= N:
        DFS(i*2)
        result += words[i]
        DFS(i*2+1)

for T in range(10):
    N = int(input())
    words = [0] * (N+1)

    for i in range(N):
        command = input().split()
        words[int(command[0])] = command[1]

    result = ""
    DFS(1)
    print(f'#{T+1} {result}')
```



# BEAKJOON 1260, 2606, 2667, 1012


​	

### 1260_DFS와 BFS

```python
import sys
from collections import deque

def DFS(V):
    print(V, end=' ')
    DFSv[V] = 1
    for i in range(1, N+1):
        if DFSv[i] == 0 and tree[V][i] == 1:
            DFS(i)
    return


def BFS(V):
    q = deque([V])
    BFSv[V] = 1
    while q:
        p = q.popleft()
        print(p, end=" ")
        for i in range(1, N+1):
            if BFSv[i] == 0 and tree[p][i] == 1:
                q.append(i)
                BFSv[i] = 1
    return
            

N,M,V = map(int, sys.stdin.readline().split())
tree = [[0]*(N+1) for _ in range(N+1)]
DFSv = [0 for _ in range(N+1)]
BFSv = [0 for _ in range(N+1)]

for _ in range(M):
    S, E = map(int, sys.stdin.readline().split())
    tree[S][E] = 1
    tree[E][S] = 1


DFS(V)
print()
BFS(V)

# 기초가 제일 중요하다.
# 혹시 DFS, BFS가 막힐 땐 이 문제를 다시 풀어보자
```

​	

### 2606_바이러스

```python
import sys

C = int(sys.stdin.readline().rstrip())
No = int(sys.stdin.readline().rstrip())

arr = [[0 for _ in range(C+1)] for _ in range(C+1)]
vistied = [0 for _ in range(C+1)]

count = 1

for _ in range(No):
    S, E = map(int, sys.stdin.readline().split())
    arr[S][E] = 1
    arr[E][S] = 1

def DFS(V):
    global count
    vistied[V] = 1
    for i in range(1, C+1):
        if vistied[i] == 0 and arr[V][i] == 1:
            vistied[i] = 1
            count += 1
            DFS(i)

DFS(count)
print(count-1)
```

​	

### 2667_단지번호붙이기

```python
import sys

def isSafe(x, y):
    if 0 <= x < N and 0 <= y < N:
        return True

def DFS(x, y, c):
    global count
    visited[x][y] = 1
    if arr[x][y] == 1:
        count += 1
    for i in range(4):
        cx = x + dx[i]
        cy = y + dy[i]      
        if isSafe(cx, cy) and visited[cx][cy] == 0 and arr[cx][cy] == 1:
            DFS(cx, cy, c)


dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

N = int(sys.stdin.readline().rstrip())

arr = [list(map(int, sys.stdin.readline().rstrip())) for _ in range(N)]
visited = [[0 for _ in range(N)] for _ in range(N)]
result = []
count = 0       # AP 단지안에 건물 숫자


for x in range(N):
    for y in range(N):
        if visited[x][y] == 0 and arr[x][y] == 1:
            DFS(x, y, count)
            result.append(count)
            count = 0

result.sort()

print(len(result))
for i in result:
    print(i)



# DFS의 나만의 규칙
# 1. 새로운 노드에 접근하자마자 방문을 표시한다.
# 2. isSafe를 만들어 범위를 확인한다.
```

​	

### 1012_유기농 배추

```python
import sys

# 최대 재귀 깊이를 늘리려면 sys 모듈의 setrecursionlimit 함수를 사용
# (기본값이상으로 안해주면 런타임에러로 처리된다.) ※기본값:1000 
sys.setrecursionlimit(50000)

def isSafe(x, y):
    if 0 <= x < N and 0 <= y < M:
        return True

def DFS(x, y):
    visited[x][y] = 1
    for i in range(4):
        cx = x + dx[i]
        cy = y + dy[i]
        if isSafe(cx, cy) and visited[cx][cy] == 0 and arr[cx][cy] == 1:
            DFS(cx, cy)

dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

result = []

for T in range(int(sys.stdin.readline().rstrip())):
    M, N, K = map(int, sys.stdin.readline().split())    # M 가로, N 세로, K 배추갯수
    
    arr = [[0 for _ in range(M)] for _ in range(N)]
    visited = [[0]*(M) for _ in range(N)]

    for i in range(K):
        y, x = map(int, sys.stdin.readline().split())
        arr[x][y] = 1

    count = 0
    for x in range(N):
        for y in range(M):
            if visited[x][y] == 0 and arr[x][y] == 1:
                DFS(x, y)
                count += 1
    result.append(count)

for i in result:
    print(i)


# 초기 풀이는 런타임 에러가 발생했다.
# 하여 결과값을 모았다가 출력하면 속도 증가가 가능하기에
# result = []에 답을 모았다가 출력해보았지만,
# 런타임 에러 발생..
# 검색해보니 
# sys.setrecursionlimit(50000) 이런 것이 있다는 것을 봤고
# 추가해주니 통과 되었다.
```

​	

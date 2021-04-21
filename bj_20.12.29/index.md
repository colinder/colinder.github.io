# BEAKJOON 2178, 7576, 1697


​	

### 2178_미로탐색

```python
import sys
from collections import deque

def isSafe(x, y):
    if 0 <= x < N and 0 <= y < M:
        return True

def BFS():
    while q:
        x, y = q.popleft()
        visited[x][y] = 1
        for i in range(4):
            cx = x + dx[i]
            cy = y + dy[i]
            if isSafe(cx, cy) and arr[cx][cy] == 1 and visited[cx][cy] == 0:
                q.append([cx, cy])
                arr[cx][cy] = arr[x][y] + 1
    

dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

N, M = map(int, sys.stdin.readline().split())

arr = [list(map(int, sys.stdin.readline().rstrip())) for _ in range(N)]
visited = [[0]*(M) for _ in range(N)]
q = deque([[0, 0]])

BFS()

print(arr[N-1][M-1])


# pop을 사용하는데 리스트형태의 자료인 경우
# x, y = [1, 2]의 모습으로도 추출이 가능
```

​	

### 7576_토마토

```python
import sys
from collections import deque

def isSafe(x, y):
    if 0 <= x < N and 0 <= y < M:
        return True


def BFS():
    while q:
        x, y = q.popleft()
        for i in range(4):
            cx = x + dx[i]
            cy = y + dy[i]
            if isSafe(cx, cy) and arr[cx][cy] == 0:
                q.append([cx, cy])
                arr[cx][cy] = arr[x][y] + 1


def result():
    global Max
    for x in range(N):
        for y in range(M):
            if arr[x][y] == 0:
                print(-1)
                return
            if arr[x][y] > Max:
                Max = arr[x][y]
    print(Max-1)
    return


dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

M, N = map(int, sys.stdin.readline().split())   # M: 6, N: 4
arr = [list(map(int, sys.stdin.readline().split())) for _ in range(N)]

q = deque()

for x in range(N):
    for y in range(M):
        if arr[x][y] == 1:
            q.append([x, y])
BFS()

Max = 0
result()


# 어떻게 하면 출발점이 2개 이상일 때
# 두 지점에서 동시에 출발해 토마토가 익는 것을 
# 판단할 수 있을까에 대한 고민이 1시간..
# 초기 코딩
# for x in range(N):
#     for y in range(M):
#         if arr[x][y] == 1:
#             q = deque([[x, y]])
#             BFS()

# 한 시간 후...
# for x in range(N):
#     for y in range(M):
#         if arr[x][y] == 1:
#             q = deque([[x, y]])
# BFS()
```

​	

### 1697_숨바꼭질

```python
import sys
from collections import deque

N, K = map(int, sys.stdin.readline().split())

arr = [0] * 30
q = deque([N])

def BFS():
    while q:
        v = q.popleft()
        if v == K:
            return print(arr[v])
        for i in [v-1, v+1, v*2]:
            if 0 <= i < 30 and arr[i] == 0:
                arr[i] = arr[v] + 1
                q.append(i)

BFS()


# 긴 방문 리스트를 만들어 놓고
# 수빈이가 기준점에서 부터 갈 수 있는 장소의 걸음수를 등록한다. 
# ex)
# 위지 : 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17
# 걸음1: 0 0 0 0 1 0 0 0 0  0  0  0  0  0  0  0  0      
# 걸음2: 0 0 0 2 1 2 0 0 0  2  0  0  0  0  0  0  0      5(기준)에서 갈 수 있는 위치
# 걸음3: 0 0 3 2 1 2 0 3 0  2  0  0  0  0  0  0  0      4(기준)에서 갈 수 있는 위치
# 걸음4: 0 0 3 2 1 2 3 3 0  2  0  3  0  0  0  0  0      6(기준)에서 갈 수 있는 위치
# 걸음5: 0 0 3 2 1 2 3 3 3  2  3  0  0  0  0  0  0 ..3  10(기준)에서 갈 수 있는 위치
# 요런 식으로 진행하면 된다.
```

​	



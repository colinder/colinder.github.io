# SW Expert Academy_D4 3143, 10966, 11592


​	

### D4_3143_가장 빠른 문자열 타이핑

```python
for T in range(int(input())):
    A, B = input().split()
    result = len(A) - (A.count(B)*(len(B)-1))
            
    print(f'#{T+1} {result}')
    
# 왜 D4 인가.
```

​	

### D4_10966_물놀이를 가자

```python
from collections import deque

dx = [0, 1, 0, -1]
dy = [1, 0, -1, 0]

for T in range(int(input())):
    N, M = map(int, input().split())
    arr = [input() for _ in range(N)]
    visited = [[-1 for _ in range(M)] for _ in range(N)]

    q = deque([])
    for i in range(N):
        for j in range(M):
            if arr[i][j] == "W":
                q.append([i,j])
                visited[i][j] = 0
    
    total = 0
    while q:
        x, y = q.popleft()
        for i in range(4):
            cx = x + dx[i]
            cy = y + dy[i]            
            if 0<= cx <N and 0<= cy <M and visited[cx][cy] == -1:
                visited[cx][cy] = visited[x][y] + 1
                q.append([cx, cy])
                total += visited[cx][cy]

    print(f'#{T+1} {total}')

# 오랜만에 다중 시작점 BFS
```

​	

### D4_11592_크루즈 컨트롤

```python
for T in range(int(input())):
    D, N = map(int, input().split())    # D: 총 거리 
    S = []

    for _ in range(N):
        k, s = map(int, input().split())    # k : 위치 / s : 속도
        S.append((D-k) / s)
    
    a = max(S)
    
    print(f'#{T+1} {D/a}')
    
# D2 난이도...
```

​	

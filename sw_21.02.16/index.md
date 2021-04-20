# SW Expert Academy_D4 3143, 10966, 11592, 11545


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

### D4_11545_틱택톰

```python
def check():
    # 가로 판단
    for i in range(4):
        if '.' not in arr[i] and 'O' not in arr[i]:
            return 'X won'
        if '.' not in arr[i] and 'X' not in arr[i]:
            return 'O won'
        
        # 세로 판단을 위한 리스트 생성
        col = [arr[j][i] for j in range(4)]
        if '.' not in col and 'O' not in col:
            return 'X won'
        if '.' not in col and 'X' not in col:
            return 'O won'
 
    # 우상단 대각선 판단
    rUp = [arr[i][i] for i in range(4)]
    if '.' not in rUp and 'O' not in rUp:
        return 'X won'
    elif '.' not in rUp and 'X' not in rUp:
        return 'O won'
    
    # 좌상단 대각선 판단
    lUP = [arr[i][3-i] for i in range(4)]
    if '.' not in lUP and 'O' not in lUP:
        return 'X won'
    elif '.' not in lUP and 'X' not in lUP:
        return 'O won'
 
    # 그밖의 경우.
    for i in range(4):
        for j in range(4):
            if arr[i][j] == '.':
                return 'Game has not completed'
 
    return 'Draw'

N = int(input())
for T in range(N):
    arr = [list(input()) for _ in range(4)]
    if T < N-1:
        _ = input()
        
    print(f'#{T+1} {check()}')

# T의 숫자를 고려하는 것에 애먹었으나, 
# 문제에 T는 최대 1개로 설정. 
# 즉, T의 숫자를 별도로 카운트 하지 않아도 됨.
```

​	



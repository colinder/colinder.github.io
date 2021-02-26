# BEAKJOON 5052, 9372


​	

### 5052_전화번호 목록

```python
import sys

for T in range(int(sys.stdin.readline().rstrip())):
    N = int(sys.stdin.readline().rstrip())

    Numbers = []
    for _ in range(N):
        Numbers.append(sys.stdin.readline().rstrip())

    Numbers.sort()

    result = "YES"
    for i in range(len(Numbers)-1):
        if Numbers[i+1].find(Numbers[i], 0, len(Numbers[i])) != -1:
            result = 'NO'
            break

    print(result)

    
# 흠.. 다른 사람들은 트리로 풀었나..?
# 이게 왜 트리에 있지...
```

​	

### 9372_상근이의 여행

```python
import sys
from collections import deque

def BFS(i):
    q = deque([i])
    visited = [True] + [False] * N
    flight = -1
    while q:
        x = q.popleft()
        if visited[x] == False:
            visited[x] = True
            flight += 1
            for j in range(len(tree[x])):
                if visited[tree[x][j]] == False:
                    q.append(tree[x][j])
    return flight
            
for T in range(int(sys.stdin.readline().rstrip())):
    N, M = map(int, sys.stdin.readline().split())

    tree = [[] for _ in range((N+1))]
    for _ in range(M):
        a, b = map(int, sys.stdin.readline().split())
        tree[a].append(b)
        tree[b].append(a)


    print(BFS(1))
```

​	



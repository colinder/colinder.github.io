# BEAKJOON 15649, 15650


​	

### 15649_N과 M (1)

```python
N, M = map(int, input().split())

def DFS(count):
    if count == M:
        print(*arr)
        return

    for i in range(N):
        if visited[i] == True:
            continue

        visited[i] = True
        arr.append(num_list[i])
        DFS(count+1)
        arr.pop()
        visited[i] = False

num_list = [i + 1 for i in range(N)]
visited = [False] * N
arr = []

DFS(0)
```

​	

### 15650_N과 M (2)

```python
import sys

def DFS(count):
    if count == M:
        print(*arr)
        return

    for i in range(N):
        if visited[i] == True:
            continue

        visited[i] = True
        arr.append(num_list[i])
        DFS(count+1)
        arr.pop()

        for j in range(i+1, N):
            visited[j] = False

N, M = map(int, sys.stdin.readline().split())

num_list = [i+1 for i in range(N)]
visited = [False] * N
arr = []

DFS(0)
```

​	

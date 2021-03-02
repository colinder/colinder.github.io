# BEAKJOON 2630


​	

### 2630_색종이 만들기

```python
import sys

def DFS(x, y, N):
    global W, B

    color = arr[x][y]
    for cx in range(x, x+N):
        for cy in range(y, y+N):
            if arr[cx][cy] != color:
                DFS(x, y, N//2)
                DFS(x, N//2+y, N//2)
                DFS(N//2+x, y, N//2)
                DFS(N//2+x, N//2+y, N//2)
                return		# 이걸 안해주면 쓸모 없는 DFS에 더 들어가게 된다.
                
    if color == 0:
        W += 1
        return
    else:
        B += 1
        return

N = int(sys.stdin.readline().rstrip())

arr = [list(map(int, sys.stdin.readline().split())) for _ in range(N)]

W, B = 0, 0 

DFS(0, 0, N)
print(W)
print(B)

# 분할한 구역에도 동일한 규칙을 적용할 수 있는 
# 알고리즘을 만드는 것이 기본 같다.
```

​	

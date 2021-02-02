# SW Expert Academy_D4 1258


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

### 

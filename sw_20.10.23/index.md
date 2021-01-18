# SW Expert Academy_D4 6959, 4613, 6109, 


​	

### D4_6959_이상한 나라의 덧셈게임

```python
for T in range(int(input())):
    N = input()
    result = ["A", "B"]
    turn = 1
    while len(N) > 1:
        N = str(int(N[0]) + int(N[1])) + N[2:]
        turn += 1
    print(f'#{T+1} {result[turn%2]}')
    
# 단순 산수
```

​	

### D4_4613_러시아 국기 같은 깃발

```python
for T in range(int(input())):
    N, M = map(int, input().split())
    a = [input() for _ in range(N)]
    result = N * M

    for i in range(1, N - 1):
        for j in range(1, N - i):
            count = 0
            for x in range(N):
                if 0 <= x < i:
                    count += M - a[x].count('W')
                elif i <= x < i + j:
                    count += M - a[x].count('B')
                else:
                    count += M - a[x].count('R')
            if result > count:
                result = count

    print(f'#{T+1} {result}')
```

​	

### D4_6109_추억의 2048게임

```python
from collections import deque

def turn():
    tmp = [[0] * N for _ in range(N)]
    for i in range(N):
        for j in range(N):
            tmp[i][j] = arr[N-1-j][i]
    return tmp

def push():
    for y in range(N):
        q = deque([])
        for x in range(N):
            if arr[x][y] != 0:
                q.append(arr[x][y])
                arr[x][y] = 0
        i = 0
        while q:
            if len(q) >= 2:
                a, b = q.popleft(), q.popleft()
                if a == b:
                    arr[i][y] = a+b
                else:
                    arr[i][y] = a
                    q.appendleft(b)
                i += 1        
            else:
                arr[i][y] = q.popleft()
    
direction = {'up': (0, 0), 'left': (1, 3), 'down': (2, 2), 'right': (3, 1)}

for T in range(int(input())):
    N, S = input().split()
    N = int(N)
    
    arr = [list(map(int, input().split())) for _ in range(N)]
    Front, Back = direction[S]

    for i in range(Front):
        arr = turn()

    push()

    for i in range(Back):
        arr = turn()

    print(f'#{T+1}')
    for i in arr:
        print(*i)

# 하... 너무 오래 걸렸다.
# 방향 전환이 싫어서 U, D, R, L를
# 각각 구현해봤으나.. 실패하고.
# 수정해야 할 것도 너무 많아지고..
# 방향 전환 구현하니까.. 이리 저리 돌리는 숫자 
# 생각하는데 또 오래 걸리고...ㅎ
```

​	

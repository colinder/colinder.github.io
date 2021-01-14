# BEAKJOON 1912, 9184, 11053, 11054


​	

### 1912_연속합

```python
import sys

N = int(sys.stdin.readline().rstrip())
arr = list(map(int, sys.stdin.readline().split()))

result = [arr[0]]
for i in range(N-1):
    result.append(max(result[i]+arr[i+1], arr[i+1]))

print(max(result))

# dp 연습하기 좋은 문제
```

​	

### 9184_신나는 함수 실행

```python
def w(a,b,c):

    if a <= 0 or b <= 0 or c <= 0:
        return 1

    if a > 20 or b > 20 or c > 20:
        return w(20, 20, 20)

    if dp[a][b][c]:
        return dp[a][b][c]
    
    if a < b and b < c:
        dp[a][b][c] = w(a, b, c-1) + w(a, b-1, c-1) - w(a, b-1, c) 
        return dp[a][b][c]

    dp[a][b][c] = w(a-1, b, c) + w(a-1, b-1, c) + w(a-1, b, c-1) - w(a-1, b-1, c-1)   
    return dp[a][b][c]

dp = [[[0]*21 for _ in range(21)] for _ in range(21)]

while True:
    a, b, c = map(int, input().split())

    if a == -1 and b == -1 and c == -1:
        break

    print(f'w({a}, {b}, {c}) = {w(a, b, c)}')

# 갑자기 난이도가 높아진듯..
# 3차원 dp까지는 어찌저찌 고안했으나
#     if dp[a][b][c]:
#         return dp[a][b][c]
# 를 어디에 넣어야 할지 고민하는데 
# 시간을 소요.
```

​	

### 11053_가장 긴 증가하는 부분 수열

```python
n = int(input())
a = list(map(int, input().split()))
dp = [0 for i in range(n)]
for i in range(n):
    for j in range(i):
        if a[i] > a[j] and dp[i] < dp[j]:
            dp[i] = dp[j]
    dp[i] += 1

print(max(dp))
```

​	

### 11054_가장 긴 바이토닉 부분 수열

```python
import sys

N = int(sys.stdin.readline().rstrip())
arr = list(map(int, sys.stdin.readline().split()))

idp = [0 for _ in range(N)]
ddp = [0 for _ in range(N)]
result = [0 for _ in range(N)]

for i in range(N):
    for j in range(i):
        if arr[i] > arr[j] and idp[i] < idp[j]:
            idp[i] = idp[j]
    idp[i] += 1

for i in range(N-1, -1, -1):
    for j in range(N-1, i, -1):
        if arr[i] > arr[j] and ddp[i] < ddp[j]:
            ddp[i] = ddp[j]
    ddp[i] += 1
    result[i] = ddp[i] + idp[i] -1

print(max(result))

# 11053을 풀어본 뒤에 도전하면 할만하다.
# range()를 거꾸로 돌리는데 살짝 애를 먹음.
```

​	

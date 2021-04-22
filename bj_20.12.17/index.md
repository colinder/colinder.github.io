# BEAKJOON 4948, 9020, 3053, 1002


​	

### 4948_베르트랑 공준

```python
import sys

while True:
    N = int(sys.stdin.readline().rstrip())

    if N == 0:
        break

    arr = [0,0] + [1] * ((2*N)-1)
    rootN = int((2*N)**0.5)

    for i in range(2, rootN+1):
        if arr[i] == 1:
            for j in range(2*i, 2*N+1, i):
                arr[j] = 0
    
    print(sum(arr[N+1:(2*N)+1]))

# 입출력에 제한이 있다가 0을 입력 받을 때 까지
# 출력을 해야 하는 조건을 달지 않는 것을 주의!
```

​	

### 9020_골드바흐의 추측

```python
import sys

# 무식한 방법....(당연히 런타임 에러)
# def checkSum():
#         dis = 10000
#         rx = 0
#         ry = 0
#         for x in range(N+1):
#             if arr[x] == 1:
#                 for y in range(x, N+1):
#                     if arr[y] == 1:
#                         if x + y == N:
#                             dif = y - x
#                             if dis > dif:
#                                 dis = dif
#                                 rx = x
#                                 ry = y
#         return rx, ry

for T in range(int(sys.stdin.readline().rstrip())):
    N = int(sys.stdin.readline().rstrip())

    # 소수 찾기
    arr = [0, 0] + [1]*(N-1)
    rootN = int(N**0.5)
    
    for i in range(2, rootN+1):
        if arr[i] == 1:
            for j in range(2*i, N+1, i):
                arr[j] = 0

    # 확인해야 하는 값을 줄이기 위해, 
    # a는 중앙에서 부터 1 방향으로 내려가고
    # b는 중앙에서 N 까지 올라가며 검증
    
    # 16을 예로 (0, 16) (1, 15) (2, 14) ...
    # 정대칭된 수를 더하면 16을 만들 수 있다. 
    # 16 만들기로 (3, 13)을 제시할 수도 있지만, 
    # 16의 중심값을 기준으로 비교하는 것이 두 수의 차이를
    # 더 줄일 수 있는 방법이었다.
    a = N//2
    b = a
    while a > 0:
        if arr[a] and arr[b]:
            print(a, b)
            break
        else:
            a -= 1
            b += 1
```

​	

### 3053_택시 기하학

```python
import sys, math

n = int(sys.stdin.readline().rstrip())

print(n**2*math.pi)     # 유클리드 기하학
print(n**2*2)           # 택시 기하학
```

​	

### 1002_터렛

```python
for T in range(int(input())):
    x1, y1, r1, x2, y2, r2 = map(int, input().split())
    distance = (((x1 - x2) ** 2) + ((y1 - y2) ** 2)) ** 0.5
    
    if x1 == x2 and y1 == y2:
        if r1 == r2:
            print(-1)
        else:
            print(0)
        continue
    
    if r1 > distance + r2 or r2 > distance + r1 or distance > r1 + r2:
        print(0)
    elif r1 == distance + r2 or r2 == distance + r1 or r1 + r2 == distance:
        print(1)
    else:
        print(2)
```

​	

# SW Expert Academy_D3 11445, 11387, 11285, 11315


​		

### D3_11445_무한 사전

```python
for T in range(int(input())): 
    P = input().rstrip()
    Q = input().rstrip()

    if P + "a" != Q:
        result = "Y"
    else:
        result = "N"

    print(f'#{T+1} {result}')
    
# 왜 D3 인가.
```

​	

### D3_11387_몬스터 사냥

```python
for T in range(int(input())):
    D, L, N = map(int, input().split())
    
    result = 0
    for i in range(N):
        result += D * (1 + L*i/100)
    
    print(f'#{T+1} {int(result)}')
```

​	

### D3_11285_다트 게임

```python
table = [20, 40, 60, 80, 100, 120, 140, 160, 180, 200]
value = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]

for T in range(int(input())):
    result = 0
    for i in range(int(input())):
        x, y = map(int, input().split())
        dis = (x**2 + y**2)**0.5
        for i, v in zip(table, value):
            if dis <= i:
                result += v
                break

    print(f'#{T+1} {result}')

# (오답 : 10000개의 테스트케이스 중 4344개가 맞았습니다.)
# 제한시간 초과가 발생하였습니다.

## python으론 안되나......
```

​	

### D3_11315_오목 판정

```python
def check(x, y):
    global result

    # 가로 검사
    for i in range(5):
        total = 0
        for j in range(5):
            total += arr[x+i][y+j]
        if total == 5:
            result = "YES"
            return
    
    # 세로 검사
    for i in range(5):
        total = 0
        for j in range(5):
            total += arr[x+j][y+i]
        if total == 5:
            result = "YES"
            return
    
    # 좌상 대각선 검사
    total = 0
    for i in range(5):
        total += arr[x+i][y+i]
    if total == 5:
        result = "YES"
        return

    # 우상 대각선 검사
    total = 0
    for i in range(5):
        total += arr[x+i][y+4-i]
    if total == 5:
        result = "YES"
        return

for T in range(int(input())):
    N = int(input())
    arr = []
    # 돌이 있으면 1, 없으면 0
    for _ in range(N):
        C = input()
        NC = []
        for i in C:
            if i == ".":
                NC.append(0)
            else:
                NC.append(1)
        arr.append(NC)

    result = "NO"    
    for x in range(N-4):
        for y in range(N-4):
            check(x, y)
    
    print(f'#{T+1} {result}')
    
# 스마트해 보이진 않는다.
```

​	

# SW Expert Academy_D3 2817, 1491, 1229, 5515


​	

### D3_2817_부분 수열의 합

```python
import itertools

for T in range(int(input())):
    N, K = map(int, input().split())   # N: 갯수,   K: 목표 합
    Nums = list(map(int, input().split()))

    count = 0
    for i in range(1, N+1):
        boxs = itertools.combinations(Nums, i)
        for box in boxs:
            if sum(box) == K:
                count += 1

    print(count)

# 하... 라이브러리 쓰자..
```

​	

### D3_1491_원재의 벽 꾸미기

```python
for T in range(int(input())):
    N, A, B = map(int, input().split())
    result = []
    for R in range(1, N+1):
        for C in range(1, R+1):
            if R * C > N:
                break
            elif R * C <= N:
                result.append(A * abs(R-C) + B * (N - R*C))
    print(f"#{T+1} {min(result)}")

# A X lR – Cl + B X (N - R X C)에서
# A X lR – Cl => 양수,   B X (N - R X C) => 양수 만 가능하다.
# 또 직사각형 인테리어라고해서 R != C 라고 생각했으나,,
# R = C인 경우도 넣어줘야 답이 나왔다.
```

​	

### D3_1229_암호문2

```python
for T in range(10):
    N = int(input())
    passwords = list(map(int, input().split()))
    order = int(input())
    cmd = list(input().split())
    for i in range(len(cmd)):
        if cmd[i] == 'I':
            for j in range(int(cmd[i+2])):
                passwords.insert(int(cmd[i+1])+j, int(cmd[i+3+j]))
        elif cmd[i] == 'D':
            for j in range(int(cmd[i+2])):
                passwords.pop(int(cmd[i+1]))

    print('#{}'.format(T+1), end=' ')
    print(*passwords[0:10])
    
# 조건 자체는 어렵지 않은데, 다뤄야하는 숫자들이 많아서 결과를 확인하기 어려웠다.
```

​	

### D3_5515_2016년 요일 맞추기

```python
days= [31, 60, 91, 121, 152, 182, 213, 244, 274, 305, 335, 366]

day = 4

for T in range(int(input())):
    m, d = map(int, input().split())

    if m != 1:
        add_month = days[m-2]
    elif m == 2:
        add_month = days[1]
    else:
        add_month = 0

    result = (day + add_month + (d-1)) % 7

    print(f'#{T+1} {result}')
    
# 이건 그냥 했다. 
```



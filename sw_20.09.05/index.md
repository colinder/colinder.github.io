# SW Expert Academy_D3 2817, 1491, , 


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



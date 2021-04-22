# BEAKJOON 2156, 2565, 10844, 9251


​	

### 2156_포도주 시식

```python
N = int(input())
wine = [0 for _ in range(N)]
total = [0 for _ in range(N)]

for i in range(N):
    wine[i] = int(input())

if N == 1:
    total[0] = wine[0]
else:
    total[0] = wine[0]
    total[1] = wine[0] + wine[1]

for i in range(2, N):
    total[i] = max(total[i-1], total[i-2] + wine[i], total[i-3] + wine[i] + wine[i-1])

print(total[N-1])

# N == 1인 경우를 생각하지 않았을 땐 런타임 오류에 걸렸었다.
# 그리고 그걸 생각하는데 오래 걸렸다.
```

​	

### 2565_전깃줄

```python
n = int(input())
line = [list(map(int, input().split())) for _ in range(n)]
line.sort(key=lambda x: x[0])  # 앞 숫자 기준, 오름차순 정리

arr = [0] * 501

for s, e in line:
    arr[e] = max(arr[:e]) + 1
    
print(n - max(arr))

# dp를 활용해 e에 겹쳐지지 않은 줄의 수를 기록
# s는 오름차순으로 정렬해 검증함으로 겹쳐지는 것을
# 신경 쓰지 않아도 ok
```

​	

### 10844_쉬운 계단 수

```python
# 진행중...
```

​	

### 9251_LCS

```python
# 진행중...
```

​	



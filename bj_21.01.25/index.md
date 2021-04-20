# BEAKJOON 1931, 11399, 1541, 13305


​	

### 1931_회의실 배정

```python
import sys

N = int(sys.stdin.readline().rstrip())

arr = [list(map(int, sys.stdin.readline().split())) for _ in range(N)]
schedule = sorted(arr, key=lambda x: (x[1], x[0]))

count, end = 0, 0
for s, f in schedule:
    if s >= end:
        count += 1
        end = f

print(count)

# 왠지 정렬을 잘하면 계산이 쉬울 것 같아서. 
# 시작시간과 종료시간을 정렬해 계산했는데
# 1. 종료시간, 2. 시작시간 순위로 정렬해야 했다.
```

​	

### 11399_ATM

```python
import sys

N = int(sys.stdin.readline().rstrip())
TT = list(map(int, sys.stdin.readline().split()))

TT = sorted(TT)

result = 0
for i, v in zip(range(N, -1, -1), TT):
    result += i*v

print(result)
```

​	

### 1541_잃어버린 괄호

```python
import sys

text = list(sys.stdin.readline().rstrip().split('-'))

Nums = []
for N in text:
    n = sum(list(map(int, N.split("+"))))
    Nums.append(n)
    
result = 0
for i in range(len(Nums)):
    if i != 0:
        result -= Nums[i]
    else:
        result += Nums[i]

print(result)


# 55-50+40 경우 결국 - 뒤에 +는 모두 묶어주면 된다.
# 예를 들어 55-50+40-30+49 경우
# 55-(50+40)-(30+49) 가 정답이 된다. 
# 즉 55 / 50+40 / 30+49 로 나누고
# 첫 인자를 제외한 나머지의 합을 빼주면 된다. 
```

​	

### 13305_주유소

```python
import sys

N = int(sys.stdin.readline().rstrip())
distance = list(map(int, sys.stdin.readline().split()))    #d  2  3  1
price = list(map(int, sys.stdin.readline().split()))       #p 5  2  4  1

total = 0
Min = sys.maxsize
    
for i in range(N-1):
    if i == 0:
        total += price[0]*distance[0]
        Min = min(Min, price[0])
    else:
        Min = min(Min, price[i])
        total += Min*distance[i]
print(total)
```

​	

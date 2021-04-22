# BEAKJOON 10989, 11650, 2108, 10814


​	

### 10989_수 정렬하기3

```python
import sys

# 시간 초과
# from collections import deque
#
# result = deque()
# for T in range(int(sys.stdin.readline().rstrip())):
#     n = int(sys.stdin.readline().rstrip())
#     if len(result) != 0:
#         for i in range(len(result)):
#             if result[i] > n:
#                 result.insert(i, n)
#                 break
#     else:
#         result.append(n)
            
# for i in result:
#     print(i)

# 속도엔 DP지.
arr = [0] * 10001
for T in range(int(sys.stdin.readline().rstrip())):
    n = int(sys.stdin.readline().rstrip())
    arr[n] += 1

for i in range(10001):
    if arr[i] != 0:
        for j in range(arr[i]):
            print(i)
```

​	

### 11650_좌표 정렬하기

```python
import sys
from collections import deque

T = int(sys.stdin.readline().rstrip())
L = deque()
for _ in range(T):
    x, y = map(int, sys.stdin.readline().split())
    L.append((x, y))

NL = sorted(L)

for i in range(T):
    print(f'{NL[i][0]} {NL[i][1]}')

# sorted는 여러 그룹의 값이 주어진 경우
# 순차적으로 증감을 비교해 준다.
# ex)
# A = [(1,2,1), (1,2,2,3), (1,1,3), (1,1,2)]
# A = sorted(A)
# print(A)    # [(1, 1, 2), (1, 1, 3), (1, 2, 1), (1, 2, 2, 3)]
```

​	

### 2108_통계학

```python
import sys
from collections import deque, Counter

result = deque()
T = int(sys.stdin.readline().rstrip())
for _ in range(T):
    result.append(int(sys.stdin.readline().rstrip()))

result = sorted(result)

def Am():
    return round(sum(result)/T)

def Me():
    return result[int(T//2)]
    

def Mo():
    MoResult = Counter(result).most_common()    # MoResult = [(-2, 1), (1, 1), (2, 1), (3, 1), (8, 1)]
    if len(MoResult) > 1:
        if MoResult[0][1] == MoResult[1][1]:
            return MoResult[1][0]
        else:
            return MoResult[0][0]
    else:
        return MoResult[0][0]

def Ra():
    return result[-1] - result[0]

print(Am())
print(Me())
print(Mo())
print(Ra())

# Counter 함수와 더 익숙해지면 좋을 것 같다.
# ex)

# from collections import Counter
# MoResult = Counter([7, 1, 2, 5, 1, 8, 7, 6]).most_common()
# print(MoResult)		# [(7, 2), (1, 2), (2, 1), (5, 1), (8, 1), (6, 1)]

# .most_common()
# 입력된 인자들의 '순서'를 존중하면서, '중복 count해줌'과 동시에 '중복 삭제'까지 진행.
```

​	

### 10814_나이순 정렬

```python
import sys
from collections import deque

T = int(sys.stdin.readline().rstrip())
L = deque()
for i in range(T):
    age, name = sys.stdin.readline().split()
    L.append((int(age), i, name))

NL = sorted(L)

for i in range(T):
    print(f'{NL[i][0]} {NL[i][2]}')
```

​	

# BEAKJOON 2798, 7568, 1018, 1436


​	

### 2798_블랙잭

```python
import sys

N, M  = map(int, sys.stdin.readline().split())
cards = list(map(int, sys.stdin.readline().split()))

result = 0
for i in range(N):
    for j in range(i+1, N):
        for x in range(j+1, N):
            if cards[i] + cards[j] + cards[x] <= M:
                result = max(result, cards[i] + cards[j] + cards[x])

print(result)
```

​	

### 7568_덩치

```python
import sys
from collections import deque

N = int(sys.stdin.readline().rstrip())

# 내가 생각한 무식한 방법...
W = deque()
H = deque()
for _ in range(N):
    x, y = sys.stdin.readline().split()
    W.append(x)
    H.append(y)

for i in range(N):
    w = W.popleft()
    h = H.popleft()

    count = 0
    for j in range(N-1):
        if W[j] > w and H[j] > h:
            count += 1
    print(count+1, end=" ")
    
    W.append(w)
    H.append(h)


# 인터넷에 있던 멋진 방법
# li = deque()

# for _ in range(N):
#     x, y = sys.stdin.readline().split()
#     li.append((x, y))

# for i in li:
#     count = 0
#     for j in li:
#         if i[0] < j [0] and i[1] < j[1]:
#             count += 1
#     print(count+1, end=" ")


# 하지만 내가 짠 코드가 더 빠르다.
```

​	

### 1018_체스판 다시 칠하기

```python
import sys
from collections import deque

N, M = map(int, sys.stdin.readline().split())

arr = [ list(sys.stdin.readline().rstrip()) for _ in range(N) ]

result = deque()
for i in range(N-7):
    for j in range(M-7):
        caseW = 0   # 시작(기준)점이 W 일 때 바뀌어야 하는 수
        caseB = 0   # 시작(기준)점이 B 일 때 바뀌어야 하는 수
        for x in range(i, i+8):
            for y in range(j, j+8):
                if (x+y) % 2 == 0:
                    if arr[x][y] == "B":
                        caseW += 1
                    if arr[x][y] == "W":
                        caseB += 1
                else:
                    if arr[x][y] == "W":
                        caseW += 1
                    if arr[x][y] == "B":
                        caseB += 1
        result.append(caseW)
        result.append(caseB)

print(min(result))
                

# 체스판에 시작점이 W로 시작했을 때 바뀌어야 하는 수
# VS 시작점이 B로 시작했을 때 바뀌어야 하는 수를 
# 비교해야 한다!
```

​	

### 1436_영화감독 숌

```python
import sys

N = int(sys.stdin.readline().rstrip())
X = 666

while N:
    if '666' in str(X):
        N -= 1
    X += 1

print(X-1)

# 666, 1666, 2666, 3666, 4666, 5666, 6661, 6662, 6663, 6664, 6665, 6667, 6668, 6669, 7666, 8666, 9666, 10666 .....
# 666이 첫 번째 수 인데 그다음 666이 들어간 수를
# 단순히 +1 하면서 찾는 것이다.
# 당연히 런타임 오류일 줄 알았으나. 아니었다.(왜 아니지...)
```

​	

# BEAKJOON 18258, 2164, 11866, 1966


​	

### 18258_큐 2

```python
import sys
from collections import deque
# import time

N = int(sys.stdin.readline().rstrip())
# start = time.time()  # 시작 시간 저장

q = deque()

for _ in range(N):
    command = sys.stdin.readline().split() 
    if command[0] == "push":
        q.append(command[1])
    elif command[0] == "pop":
        if q:
            print(q.popleft())
        else:
            print(-1)
    elif command[0] == "size":
        print(len(q))
    elif command[0] == "empty":
        if not q:
            print(1)
        else:
            print(0)
    elif command[0] == "front":
        if q:
            print(q[0])
        else:
            print(-1)
    elif command[0] == "back":
        if q:
            print(q[len(q)-1])
        else:
            print(-1)

# print("time :", time.time() - start)  # 현재시각 - 시작시간 = 실행 시간


# q[len(q)-1] VS q[-1] 속도 비교
# q[len(q)-1] 이 더 빠름.

```

​	

### 2164_카드2

```python
import sys
from collections import deque

N = int(sys.stdin.readline().rstrip())

q = deque([i+1 for i in range(N)])

while len(q) != 1:
    q.popleft()
    q.append(q.popleft())

print(q[0])

```

​	

### 11866_요세푸스 문제 0

```python
import sys
from collections import deque

N, K = map(int, sys.stdin.readline().split())

q = deque([i+1 for i in range(N)])
result = []

while q:
    q.rotate(-K+1)
    result.append(q.popleft())

print("<", end="")
for i in range(N):
    if i != N-1:
        print(result[i], end=", ")
    else:
        print(result[i], end="")
print(">")

```

​	

### 1966_프린터 큐

```python
import sys
from collections import deque

for T in range(int(input())):
    N, M = map(int, sys.stdin.readline().split())
    q = deque(map(int, sys.stdin.readline().split()))
    idx = deque([0 for i in range(N)])
    idx[M] = 1
    count = 0
    while True:
        if q[0] == max(q):
            count += 1
            if idx[0] == 1:
                print(count)
                break
            else:
                q.popleft()
                idx.popleft()
        else:
            q.append(q.popleft())
            idx.append(idx.popleft())

# 큐를 사용하는 알고리즘을 고민하는데
# 아주 좋은 문제!!
```

​	



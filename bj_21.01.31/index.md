# BEAKJOON 1021, 5430, 10866


​	

### 1021_회전하는 큐

```python
import sys
from collections import deque

N, M = map(int, sys.stdin.readline().split())
arr = deque([i for i in range(1, N+1)])
poplist = deque(list(map(int, sys.stdin.readline().split())))

count = 0
while poplist:
    try:
        if arr[0] == poplist[0]:
            arr.popleft()
            poplist.popleft()
        L = len(arr)
        a = abs(arr.index(arr[0]) - arr.index(poplist[0]))
        b = L - a
        if a < b:
            arr.rotate(-a)
            count += a
        elif a >= b:
            arr.rotate(b)
            count += b
    except IndexError:
        break

print(count)
```

​	

### 5430_AC

```python
import sys
from collections import deque

for T in range(int(sys.stdin.readline().rstrip())):
    P = sys.stdin.readline().rstrip()
    N = int(sys.stdin.readline().rstrip())
    ## 다양하게 input 받는 거 고민하기 좋다.
    arr = deque(list(sys.stdin.readline().rstrip()[1:-1].split(",")))

    if N == 0 or len(arr) == 0:
        arr = deque([])

    reverse = False
    error = False
    for i in P:
        if i == "R":
            reverse =  not reverse
        elif i == "D":
            if len(arr) == 0:
                error = True
                break
            if reverse:
                arr.pop()
            else:
                arr.popleft()
    if error:
        print("error")
    else:
        if reverse:
            arr.reverse()
        print("[", end="") 
        print(",".join(arr), end="") 
        print("]")


    ## reverse() 여러번 돌리면 시간초과 뜬다. 
    # flag = True
    # for i in P:
    #     if i == "R":
    #         arr.reverse()
    #     else:
    #         if len(arr) != 0:
    #             arr.popleft()
    #         else:
    #             print("error")
    #             flag = False
    #             break
    
    # if flag:
    #     if len(arr) != 0:

    #         print("[", end="") 
    #         print(",".join(arr), end="") 
    #         print("]")
    #     else:
    #         print('error')
```

​	

### 10866_덱

```python
import sys
from collections import deque

N = int(sys.stdin.readline().rstrip())
q = deque()
for i in range(N):
    command = sys.stdin.readline().split()
    if command[0] == "push_front":
        q.appendleft(command[1])
    elif command[0] == "push_back":
        q.append(command[1])
    elif command[0] == "pop_front":
        if q:
            print(q.popleft())
        else:
            print(-1)
    elif command[0] == "pop_back":
        if q:
            print(q.pop())
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
```

​	



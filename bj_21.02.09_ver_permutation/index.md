<script src="/livereload.js?mindelay=10&amp;v=2&amp;port=1313&amp;path=livereload" data-no-instant defer></script># BEAKJOON 15649 Ver_permutation


​	

### 15649_N과 M (1)

```python
import sys, itertools

N, M = map(int, sys.stdin.readline().split())

arr = []
for i in range(1, N+1):
    arr.append(str(i))

Narr = list(map(" ".join, itertools.permutations(arr, M)))
print("\n".join(Narr))


## join 정리
# "join"은 기본적으로 "list의 인자"를 "string으로 변환"할 때 사용한다.
# ex)
# ":".join(변환할 리스트)  =>  변환할 리스트의 인자를 string으로 변환하는데 인자 사이 마다 : 을 넣겠다.


## permutation 정리
# a = ["1", "2", "3"]
# b = list(itertools.permutations(a, 2))
# print(b)    # [('1', '2'), ('1', '3'), ('2', '1'), ('2', '3'), ('3', '1'), ('3', '2')]

# a = ["1", "2", "3"]
# b = map(" ".join, itertools.permutations(a, 2))
# print(b)    # <map object at 0x0000021D29F0AC40>

# # list의 경우
# a = ["1", "2", "3"]
# b = list(map(" ".join, itertools.permutations(a, 2)))
# print(b)    # ['1 2', '1 3', '2 1', '2 3', '3 1', '3 2']


# # tuple의 경우
# a = ["1", "2", "3"]
# b = list(map(" ".join, itertools.permutations(a, 2)))
# print("tuple: ", b)    # ['1 2', '1 3', '2 1', '2 3', '3 1', '3 2']


# # set의 경우(set은 순서가 없다.)
# a = set(["1", "2", "3"])
# b = list(itertools.permutations(a, 2))
# print("set: ", b)    # [('2', '3'), ('2', '1'), ('3', '2'), ('3', '1'), ('1', '2'), ('1', '3')]


# # dict의 경우(dict는 순서가 없다.)
# a = {"1", "2", "3"}
# b = list(itertools.permutations(a, 2))
# print("dict: ", b)    # [('3', '1'), ('3', '2'), ('1', '3'), ('1', '2'), ('2', '3'), ('2', '1')]
```

​	

### 15650_N과 M (2)

```python
import sys, itertools
from collections import deque

Q = deque()
for i in range(1, N+1):
    Q.append(str(i))

NQ = deque(map(" ".join, itertools.combinations(Q, M)))
print("\n".join(NQ))
```

​	

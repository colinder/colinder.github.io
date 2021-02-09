# BEAKJOON 15649


### 15649_N과 M(1)

```python
N, M = map(int, input().split())

def DFS(count):
    if count == M:
        print(*arr)
        return

    for i in range(N):
        if visited[i] == True:
            continue

        visited[i] = True
        arr.append(num_list[i])
        DFS(count+1)
        arr.pop()
        visited[i] = False

num_list = [i + 1 for i in range(N)]
visited = [False] * N
arr = []

DFS(0)

# 내부 메서드로도 해결 가능. 하지만.. 백트래킹 연습해보자

# import sys, itertools


# N, M = map(int, sys.stdin.readline().split())

# arr = []
# for i in range(1, N+1):
#     arr.append(str(i))
# print(arr)

# Narr = list(map(" ".join, itertools.permutations(arr, M)))
# print("\n".join(Narr))


## join 정리
# "join"은 기본적으로 "list의 인자"를 "string으로 변환"할 때 사용한다.
# ex)
# ":".join(변환할 리스트)  =>  변환할 리스트의 인자를 string으로 변환하는데 인자 사이 마다 : 을 넣겠다.

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

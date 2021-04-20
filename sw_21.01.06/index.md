# SW Expert Academy_D3 5678, 5672, 7465, 7701


​	

### D4_5678_[Professional] 팰린드롬

```python
for T in range(int(input())):
    S = " " + input()
    L = len(S)
    result = 1
    for i in range(2, L):
        for j in range(L-i):
            if S[j+1:j+i+1] == S[j+i:j:-1]:
                result = i

    print(f'#{T+1} {result}')
    
# 슬라이싱을 잘 고민 하면 쉽게 해결
```

​	

### D4_5672_[Professional] 올해의 조련사

```python
for T in range(int(input())):
    q = []
    n = ""
    for N in range(int(input())):
        q.append(input())

    S = 0
    L = N

    while S != L:
        if q[S] < q[L]:
            n += q[S]
            S += 1
        elif q[S] > q[L]:
            n += q[L]
            L -= 1
        else:
            W = int((L-S)//2)
            for i in range(W):
                flag = True
                c = q[S+i+1]
                d = q[L-(i+1)]
                if c == d:
                    continue
                elif c < d:
                    n += q[S]
                    S += 1
                    flag = False
                    break
                elif c > d:
                    n += q[L]
                    L -= 1
                    flag = False
                    break
            if flag:
                n += q[S]
                S += 1
    n += q[L]

    print(f'#{T+1} {n}')

# 조건은 다 쉬웠으나, 검사하는 횟수?에 착각을해
# 생각보다 시간이 많이 걸린 문제
```

​	

### D4_7465_창용 마을 무리의 개수

```python
def DFS(x):
    visited[x] = 1
    for i in range(1, N+1):
        if arr[x][i] == 1 and visited[i] == 0:
            DFS(i)

for T in range(int(input())):
    N, M = map(int, input().split())
    arr = [[0 for _ in range(N+1)] for _ in range(N+1)]
    visited = [0 for _ in range(N+1)]

    for i in range(M):
        x, y = map(int, input().split())
        arr[x][y] = 1
        arr[y][x] = 1

    count = 0
    for i in range(1, N+1):
        if visited[i] == 0:
            DFS(i)
            count += 1

    print(f'#{T+1} {count}')

# 양방향입니다.
```

​	

### D4_7701_염라대왕의 이름 정렬

```python
for T in range(int(input())):
    N = int(input())
    S = set()
    for i in range(N):
        s = input()
        S.add(s)
    
    A = sorted(S, key=lambda x: (len(x), x))
    
    print(f'#{T+1}')
    for j in A:
        print(j)

# set 자료형도 sorted를 진행하면
# 자동으로 list type으로 변한다.
```

​	

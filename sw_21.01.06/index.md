# SW Expert Academy_D3 5678, 5672


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


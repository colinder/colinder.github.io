# SW Expert Academy_D4 6959, 4613, , 


​	

### D4_6959_이상한 나라의 덧셈게임

```python
for T in range(int(input())):
    N = input()
    result = ["A", "B"]
    turn = 1
    while len(N) > 1:
        N = str(int(N[0]) + int(N[1])) + N[2:]
        turn += 1
    print(f'#{T+1} {result[turn%2]}')
    
# 단순 산수
```

​	

### D4_4613_러시아 국기 같은 깃발

```python
for T in range(int(input())):
    N, M = map(int, input().split())
    a = [input() for _ in range(N)]
    result = N * M

    for i in range(1, N - 1):
        for j in range(1, N - i):
            count = 0
            for x in range(N):
                if 0 <= x < i:
                    count += M - a[x].count('W')
                elif i <= x < i + j:
                    count += M - a[x].count('B')
                else:
                    count += M - a[x].count('R')
            if result > count:
                result = count

    print(f'#{T+1} {result}')
```

​	

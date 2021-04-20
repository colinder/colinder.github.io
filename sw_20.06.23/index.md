# SW Expert Academy_D3 6190, 6057, 6019, 5986


​	

### D3_6190_정곤이의 단조 증가하는 수

````python
for T in range(int(input())):
    N = int(input())
    L = list(map(int, input().split()))

    result = -1
    for i in range(N-1):
        for j in range(i+1, N):
            num = str(L[i]*L[j])
            if len(num) > 1 and '0' not in num and result < int(num) and list(num) == sorted(num):
                result = int(num)
    print(f'#{T+1} {result}')

# 단조증가하는 지를 검증하는 방법으로 sorted를 썼는데 이를 활용하기 위해서는 str형태로 변경해야 검증이 가능하다.
# 한자리 숫자는 단조증가하는 수가 아니다.
# 중간에 0이 있다면 볼 필요도 없이 단조증가수가 아니기 때문에 처리 속도 증가가 가능하다. 
````

​	

### D3_6057_그래프의 삼각형

```python
for T in range(int(input())):
    N, M = map(int,input().split())     # N: 정점, M: 간선
    arr = [[0]*(N+1) for n in range(N+1)]
    # part1
    for _ in range(M):
        X, Y = map(int,input().split())
        arr[X][Y] = 1
        arr[Y][X] = 1

    # part2
    result = 0
    for i in range(1, N+1):
        for j in range(i+1, N+1):
            if arr[i][j] == 1:
                for r in range(j+1, N+1):
                    if arr[j][r] == 1 and arr[r][i] == 1:
                        result += 1
    print(f"#{T+1} {result}")

# part1에서 삼각형의 정점의 정보를 담는 arr을 만들고
# part2에서 만들어진 배열을 돌며 정점의 정보를 발견(1)하면 해당 정점을 기준으로 삼각형이 되는지 검증
```

​	

### D3_6019_기차 사이의 파리

```python
for T in range(int(input())):
    D, A, B, F = map(int,input().split())
    t = D / (B+A)
    print('#{} {}'.format(T+1, F*t))

#단순 수학으로 해결
```

​	

### D3_5986_새샘이와 세 소수

```python
for T in range(int(input())):
    N = int(input())

    # part1
    PN = set()
    for i in range(2, N+1):
        for j in range(2, i+1):
            if i % j == 0:
                break
        PN.add(j)

    PN =list(PN)
    pn = PN.sort(reverse=True)
    
	# part2
    count = 0
    for i in range(len(PN)):
        for j in range(i,len(PN)):
            for k in range(j,len(PN)):
                if N == PN[i]+PN[j]+PN[k]:
                    count += 1
    print('#{} {}'.format(T+1, count))
    
# part1에서 소수 리스트를 만들고(1과 자기 자신만을 약수로 가지는 수 리스트)
# part2에서 세 소수의 합으로 나타낼 경우의 수를 종합.
```



# SW Expert Academy_D3 10505, 10200, 3376, 5642


​	

### D3_10505_소득 불균형

```python
for T in range(int(input())):
    N = int(input())
    dp = [0] * 100001
    Max = 0
    for i in list(map(int, input().split())):
        dp[i] += 1
        Max += i

    print(f'#{T+1} {sum(dp[:int(Max/N)+1])}')
```

​	

### D3_10200_구독자 전쟁

```python
for T in range(int(input())):
    N, A, B = map(int, input().split())
    # 최소값 구하기
    if A+B >= N:
        Min = (A+B)-N
        if A >= B:
            Max = B
        else:
            Max = A
    else:
        Min = 0
        if A >= B:
            Max = B
        else:
            Max = A

    print(f'#{T+1} {Max} {Min}')
```

​	

### D3_3376_파도반 수열

```python
for T in range(int(input())):
    Padovan = [1, 1, 1, 2]
    N = int(input())
    for i in range(N-4):
        Padovan.append(Padovan[i+2]+Padovan[i+1])

    print(f'#{T+1} {Padovan[N-1]}')
```

​	

### D3_5642_합

```python
T = int(input())
for T in range(T):
    N = int(input())
    Nums = list(map(int, input().split()))

    result = 0
    Sum = 0
    flag = 0
    for i in range(N):
        Sum = Sum + Nums[i]
        if Sum < 0:
            Sum = 0
        elif Sum > result:
            result = Sum
        if Nums[i] < 0:
            flag += 1

    if flag == N:
        result = max(Nums)

    print(f'#{T+1} {result}')

# 모두 음수인 경우를 생각해야 한다... 이거 생각하는데 40분 걸렸따...
```

​	

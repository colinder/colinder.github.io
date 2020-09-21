# SW Expert Academy_D3 3307, 3304, ,


​	

### D3_3307_최장 증가 부분 수열(LIS)

```python
for T in range(int(input())):
    N = int(input())
    arr = list(map(int, input().split()))
    dp= [1] * N
    
    for i in range(1, N):
        for j in range(i):
            if arr[j] < arr[i]:
                dp[i]= max(dp[i], dp[j] + 1)

    print(f'#{T+1} {max(dp)}')

# dp는 쉬운듯 어렵고 어려운듯 쉽다...
# LIS로 검색하면 도움이 되는 글이 많다. 
```

​	

### D3_3304_최장 공통 부분 수열(LCS)

```python
for T in range(int(input())):
    A, B = input().split()
    dp = [[0 for i in range(len(A)+1)] for j in range(len(B)+1)]

    for i in range(1, len(B)+1):
        for j in range(1, len(A)+1):
            if B[i-1] == A[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i][j-1], dp[i-1][j])
                            
    print(f'#{T+1} {dp[-1][-1]}')

# 아니다. DP는 그냥 겁나 어렵다..
# 각각의 문자 위치를 숫자로 바꾸고 이를 2차원 배열로 만들어서
# 일치하는 문자가 나왔을 때의 숫자를 dp에 기록해서 진행하는데
# 난 A, B의 검사 순서를 B -> A 로 해야 한다는 것을 아주 느리게 깨달았다.
# LCS로 검색하면 도움이 되는 글이 많다. 
```

​	

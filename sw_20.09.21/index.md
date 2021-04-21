# SW Expert Academy_D3 3307, 3304, 4371, 10726


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

### D3_4371_항구에 들어오는 배

```python
for T in range(int(input())):
    N = int(input())
    days = []
    for _ in range(N):
        days.append(int(input())-1)

    days = days[1:]
    dp = [1]*(N-1)
    
    count = 0
    while any(dp):								# 👈 dp가 모두 0일 때까지 while을 돌린다.
        i = dp.index(1)
        for j in range(i,N-1):
            if days[j] % days[i] == 0:
                dp[j] = 0
        count += 1

    print(f"#{T+1} {count}")

# 2주기로 정박하는 배가 있다고 할 때, 이 배는 2일 4일 8일 12일에 정박할 수 있다.
# 중간에 비어있는 일자가 있더라도 1배가 정박한 것으로 본다.
# any() 함수를 써봤는데 좋았다. 푸는데 2시간... 걸렸다...ㅎ.
```

​		

### D3_10726_이진수 표현

```python
def check2(M):
    for i in range(N):
        if M % 2 != 1:
            return "OFF"
        else:
            M = M // 2
    return "ON"

for T in range(int(input())):
    N, M = map(int, input().split())

    print(f'#{T+1} {check2(M)}')
        
# 특정 조건에서 더 이상 검색할 필요가 없다면, def를 선언해 사용하는 것이 아주 유용하다. 
# 처음에는 bin함수를 사용해 slicing을 해서 뭐..뭐.. 어떻게 해보려 했지만
# run time error가 났다. 조금 생각해보니, 굳이 bin을 사용할 것도 없어보였고,
# 간단하게 시도했더니 풀렸다.
```

​	

# SW Expert Academy_D3 10761, 10804, 10912, 10965


​	

### D3_10761_신뢰

```python
from collections import deque

for T in range(int(input())):
    command = deque(input().split())
    N = command.popleft()
    
    B = deque()
    O = deque()
    order = deque()
    while command:
        i = command.popleft()
        if i == "B":
            B.append(int(command.popleft()))
            order.append("B")
        else:
            O.append(int(command.popleft()))
            order.append("O")

    count = 0
    b, o = 1, 1
    while True:
        if not order:
            break
        count += 1

        BP = "안눌림"
        if B:
            if B[0] > b:
                b += 1
            elif B[0] == b and order[0] == "B":
                B.popleft()
                order.popleft()
                BP = "눌림"
            elif B[0] < b:
                b -= 1
        
        if O:
            if O[0] > o:
                o += 1
            elif O[0] == o and order[0] == "O" and BP == "안눌림":
                O.popleft()
                order.popleft()
            elif O[0] < o:
                o -= 1

    print(f'#{T+1} {count}')


# 주어진 순서대로 버튼을 눌러야 한다!!!
# 첫번째 테스트의 경우 O에 1이 있으나, B2가 눌리기를 기다려야 한다.
# order 리스트가 필요한 이유
```

​	

### D3_10804_문자열의 거울상

```python
for T in range(int(input())):
    S = input()
    L = len(S)
    result = ""
    for i in range(L-1, -1, -1):
        if S[i] == "b":
            result += "d"
        elif S[i] == "d":
            result += "b"
        elif S[i] == "q":
            result += "p"
        else:
            result += "q"
    print(f'#{T+1} {result}')
```

​	

### D3_10912_외로운 문자

```python
from collections import Counter

for T in range(int(input())):
    S = Counter(input()).most_common()
    R = ""
    for i in S:
        if i[1] % 2 == 1:
            R += i[0]
    
    result = sorted(R)

    if len(result) == 0:
        print(f'#{T+1} Good')
    else:
        result2 = ""
        for i in result:
            result2 += i
        print(f"#{T+1} {result2}")

# Counter와 .most_common 은 유용하다.
```

​	

### D3_10965_제곱수 만들기

```python
P = [2]
for i in range(3,3162,2):
    for q in P:
        if i % q == 0:
            break
    else:
        P.append(i)

ans = []
for T in range(int(input())):
    N = int(input())
    result, n = 1, N
    for v in P:
        count = 0
        if n == 1 or v > N:
            break
        while n % v == 0:
            n //= v
            count += 1
        if count % 2 != 0:
            result *= v
    if n > 1:
        result *= n
    ans.append(f'#{T+1} {result}')

for i in ans:
    print(i)

# 에라토스테네스 체를 이용해 소수를 구했고
# root(10000001**0.5) 보다 큰 소수의 경우 
# 소인수 분해를 하고 나서도 n != 1 수 있다.
# 하여 소인수분해를 마치고도 n > 1면 
# result에 곱해주었다.

# 속도와 n이 root보다 큰 경우가 있다는 것을 깨닫는데
# 오랜시간이 걸렸던 문제.
```

​	



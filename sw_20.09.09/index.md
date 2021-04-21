# SW Expert Academy_D3 4698, 1493, 10570, 3032


​	

### D3_4698_테네스의 특별한 소수

```python
for T in range(int(input())):
    D, A, B = map(int, input().split())
    D = str(D)
    arr = [0, 0] + [1] * (B - 1)
    # 에라토스테네스의 체를 활용할껀데 어떤 수까지 검증하면 될까?
    # 검증해보니 모든 수는 자신의 int(root)까지만 확인하면 배수값인지 확인이 가능했다.
    rootB = int(B**0.5)							#👈 이 아이디어를 얻기까지 오랜시간이 걸렸다.

    for i in range(2, rootB+1):					#👈
        if arr[i] == 1:							#👈
            for j in range(2*i, B+1, i):		#👈 에라토스테네스의 체 를 활용
                arr[j] = 0						#👈

    result = []
    for i in range(A, B+1):
        if arr[i] == 1 and D in str(i):
            result.append(i)

    print(f'#{T+1} {len(result)}')
```

​	

### D3_1493_수의 새로운 연산

```python
new_number = [(0, 0)]
count = 1
while count <= 300:
    count += 1
    for x in range(1, count):
        y = count - x
        new_number.append((x, y))

for T in range(int(input())):
    p, q = map(int, input().split())
    x = new_number[p][0] + new_number[q][0]
    y = new_number[p][1] + new_number[q][1]
    result = new_number.index((x, y))
    print(f'#{T+1} {result}')

# 별다른 아이디어는 없었다. 그냥 인덱스별 좌표를 미리 만들어 두고 
# 그걸 불러오는 방법을 생각했다. (런타임이 겁났지만, Pass 했다.)
```

​		

### D3_10570_제곱 팰린드룸 수

```python
for T in range(int(input())):
    N = [0]*1001

    A, B = map(int, input().split())
    for i in range(A, B+1):
        if str(i) == str(i)[::-1]:
            if i**0.5 % 1 == 0:
                if str(int(i**0.5)) == str(int(i**0.5))[::-1]:
                    N[i] = 1

    print(f'#{T+1} {N.count(1)}')

#그냥 문제에 주어지는 대로 풀었다.
```

​	

### D3_3032_홍준이의 숫자 놀이

```python
def exeu(a, b):
    global x, y
    r = [a, b]
    s = [1, 0]
    t = [0, 1]

    while r[-1] != 0:
        q = int(r[-2] / r[-1])
        r.append(r[-2] - q * r[-1])
        s.append(s[-2] - q * s[-1])
        t.append(t[-2] - q * t[-1])
    x = s[-2]
    y = t[-2]

for T in range(int(input())):
    A, B = map(int, input().split())
    exeu(A, B)

    print(f'#{T+1} {x} {y}')

# 확장된 유클리드 알고리즘을 공부하면 된다.
# 근데 쉽지 않으니 시간을 가지고 봐야 한다.
# 실은 난 아직 이해하지 못했다.
```

​	

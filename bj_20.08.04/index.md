# BEAKJOON 1103, 1904, 2748, 9461


​	

### 1003_피보나치 함수

```python
def Fibonacci(N):
    if N < 3:
        print(zero[N], one[N])
    else:
        f = 1
        s = 2
        for i in range(N-2):
            zero.append(zero[f] + zero[s])
            one.append(one[f] + one[s])
            f += 1
            s += 1
        print(zero[N], one[N])

for T in range(int(input())):
    N = int(input())

    zero = [1, 0, 1]
    one = [0, 1, 1]

    Fibonacci(N)
```

​		

### 1904_01타일

```python
def Fibonacci(N):
    result: 0
    f = 0
    s = 1
    for i in range(1, N+3):
        if i == 1:
            result = f
        elif i == 2:
            result = s
        else:
            result = f+s
            f = s % 15746
            s = result % 15746
    print(result % 15746)

Fibonacci(int(input()))

# 문제를 먼저 손으로 플어보니 결국 피보나치 수열이 답이었는데
# 15746의 나머지가 답이어서 그런지 '시간 초과'가 발생했다.
# 때문에 '왜맞틀?'로 고민하다 검색해보니 결과값을 15746의 나머지로 더한다는
# 방법을 보고 이를 참고했다. (솔직이 아직 이게 왜 더 빠른지 잘 모르겠다.)
```

​		

### 2748_피보나치의 수2

```python
def Fibonacci(N):
    Fibo = [0, 1]
    f = 0
    s = 1
    for i in range(3, N+2):
        Fibo.append(Fibo[f] + Fibo[s])
        f += 1
        s += 1
    print(Fibo[N])

N = int(input())

Fibonacci(N)
```

​		

### 9461_파도반 수열

```python
for T in range(int(input())):
    N = int(input())
    Padovan = [1, 1, 1]
    f = 0
    s = 1
    for i in range(N-3):
        Padovan.append(Padovan[f] + Padovan[s])
        f += 1
        s += 1

    print(Padovan[N-1])
```

​	

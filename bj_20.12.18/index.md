# BEAKJOON 10872, 10870, 2447, 11729


​	

### 10872_팩토리얼

```python
import sys

N = int(sys.stdin.readline().rstrip())

def fac(n):
    if n > 1:
        return n * fac(n-1)
    else:
        return 1

print(fac(N))
```

​	

### 10870_피보나치 수 5

```python
import sys

def fivo(n):
    if n >= 2:
        return fivo(n-1) + fivo(n-2)
    else:
        return n

N = int(sys.stdin.readline().rstrip())

print(fivo(N))
```

​	

### 2447_별 찍기 - 10

```python
import sys

n = int(sys.stdin.readline().rstrip())

def star(i, j):
    while(i != 0):
        if i % 3 == 1 and j % 3 == 1:
            sys.stdout.write(' ')
            return 
        i //= 3
        j //= 3
    sys.stdout.write('*')

for i in range(n):
    for j in range(n):
        star(i, j)
    sys.stdout.write('\n')

# 이런 디자인은 어떻게 생각할 수 있는 건지..
# 재귀는 너무 어렵다.
```

​	

### 11729_하노이 탑 이동 순서

```python
import sys

N = int(sys.stdin.readline().rstrip())

def hanoi(N, s, m, e):
    if N == 1:
        print(s, e)
    else:
        hanoi(N-1, s, e, m)
        print(s, e)
        hanoi(N-1, m, s, e)

Sum = 1
for i in range(N - 1):
    Sum = Sum * 2 + 1
print(Sum)

hanoi(N, 1,2,3)

# 이런 디자인은 어떻게 생각할 수 있는 건지..
# 너무 어렵다.
```

​	


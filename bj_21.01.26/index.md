# BEAKJOON 1037, 1934, 2609, 5086


​	

### 1037_약수

```python
import sys

N = int(sys.stdin.readline().rstrip())
arr = list(map(int, sys.stdin.readline().split()))

Max = max(arr)
Min = min(arr)
print(Max*Min)
```

​		

### 1934_최소공배수

```python
import sys

def GCD(a, b):
    while b != 0:
        m = a % b
        a = b
        b = m
    return a

for T in range(int(sys.stdin.readline().rstrip())):
    A, B = map(int, sys.stdin.readline().split())

    gcd = GCD(A, B)
    a = A // gcd
    b = B // gcd
    print(a*b*gcd)
```

​		

### 2609_최대공약수와 최소공배수

```python
import sys

def GCD(a, b):  #최대공약수(유클리드 호제법)
    while b != 0:
        m = a % b
        a = b
        b = m
    return a
    

A, B = map(int, sys.stdin.readline().split())

gcd = GCD(A, B)

a = A // gcd
b = B // gcd            

print(gcd)
print(a*b*gcd)
```

​		

### 5086_배수와 약수

```python
import sys

while True:
    a, b = map(int, sys.stdin.readline().split())
    if a == 0 and b == 0:
        break
    if b % a == 0:
        print("factor")
    elif a % b == 0:
        print("multiple")
    elif a % b != 0 and b % a != 0:
        print("neither")
```

​		

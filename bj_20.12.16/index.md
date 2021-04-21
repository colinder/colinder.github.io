# BEAKJOON 10250, 2775, 2869, 1011


​	

### 10250_ACM호텔

```python
import sys

for T in range(int(sys.stdin.readline().rstrip())):
    H, W, N = map(int, sys.stdin.readline().split())

    quo = str((N // H) + 1)
    rem = str(N % H)

    if N % H == 0:
        quo = str(N//H)
        rem = str(H)

    print(rem+quo.zfill(2))

# zfill?
# a = "2"
# b = "12"

# A = a.zfill(2)
# B = b.zfill(2)
# print(A)	// 02
# print(B)	// 12
```

​	

### 2775_부녀회장이 될테야

```python
import sys

for T in range(int(sys.stdin.readline().rstrip())):
    floor = int(sys.stdin.readline().rstrip())
    ho = int(sys.stdin.readline().rstrip())

    floor0 = [_ for _ in range(1,ho+1)]

    for i in range(floor):
        for j in range(1, ho):
            floor0[j] += floor0[j-1]

    print(floor0[-1])

# 놀랍게도 print(floor0[j])로 제출하면
# 런타임 에러.... 변수를 사용하는 것 보다
# 인덱스로(슬라이싱?)을 사용해 접근하는 것이 더 빠른가보다.
```

​	

### 2869_달팽이는 올라가고 싶다

```python
import sys

A, B, V = map(int, sys.stdin.readline().split())

# 의식의 흐름에 따른 코딩
# day = 1
# posi = 0
# while True:
#     posi += A
#     if posi >= V:
#         break
#     else:
#         posi -= B
#     day += 1
# print(day) 

# 의식을 차리고 난 후 코딩
print((V - B - 1) // (A - B) + 1)
```

​	

### 1011_Fly me to the Alpha Centauri

```python
import sys

for T in range(int(sys.stdin.readline().rstrip())):
    x, y = map(int, sys.stdin.readline().split())
    distance = y - x
    if distance <= 3:
        print(distance)
    else:
        sr = int(distance**0.5)
        if distance == sr**2:
            print(2*sr - 1)
        elif sr**2 < distance <= sr**2 + sr:
            print(2*sr)
        else:
            print(2*sr+1)

# 일단 손으로 노가다를 해야
# 규칙이 보인다...
```

​	

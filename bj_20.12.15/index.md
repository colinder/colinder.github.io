# BEAKJOON 10809, 2941, 2908, 1316


​	

### 10809_알파벳찾기

```python
import sys

word = sys.stdin.readline().rstrip()
alphabet = list(range(97,123))

for x in alphabet :
    print(word.find(chr(x)), end=" ")

# find함수는 조건에 맞는 값의 index를 출력하고
# 찾지 못하는 경우 -1을 출력한다. 
```

​	

### 2941_크로아티아 알파벳

```python
import sys

cro = ["c=", "c-", "dz=", "d-", "lj", "nj", "s=", "z="]

s = sys.stdin.readline().rstrip()

for i in cro:
    s = s.replace(i, "*")

print(len(s))
```

​	

### 2908_상수

```python
import sys

A, B = sys.stdin.readline().split()

NA = int(A[::-1])
NB = int(B[::-1])

if NA > NB :
    print(NA)
else: 
    print(NB)
    
# 문자열 슬라이싱을 잘 활용하면 아주 좋다.
```

​	

### 1316_그룹 단어 체커

```python
import sys

N = int(sys.stdin.readline().rstrip())

for i in range(N):
    sent = sys.stdin.readline().rstrip()
    for j in range(1, len(sent)):
        if sent.find(sent[j-1]) > sent.find(sent[j]):
            N -= 1
            break
print(N)

# find함수도 잘 알아두면 좋다.
```

​	

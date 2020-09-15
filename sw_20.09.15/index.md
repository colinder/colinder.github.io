# SW Expert Academy_D3 3131, 3233, , 


​	

### D3_3131_100만 이하의 모든 소수

```python
a = [0, 0] + [1] * 1000000

for i in range(2, 1000000):
    if a[i] == 1:
        for j in range(2*i, 1000001, i):
            a[j] = 0

for i in range(2, 1000000):
    if a[i] == 1:
        print(i, end=" ")
    
# 에라토스테네스의 체
```

​	

### D3_3233_정삼각형 분할 놀이

```python
for T in range(int(input())):
    A, B = map(int, input().split())

    result = 0
    for i in range(int(A//B)):
        result += 2*(i+1) - 1

    print(f'#{T+1} {result}')
    
# A를 B의 길이로 나눈 정삼각형은 맨 위층 부터 2n-1개의 삼각형이 나온다. 
```

​	

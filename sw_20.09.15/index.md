# SW Expert Academy_D3 3131, 3233, 1860, 4047


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

### D3_1860_진기의 최고급 붕어빵

```python
def check():
    global bread
    for i in range(max(people)+1):
        if i == 0:
            pass
        else:
            if i % M == 0:
                bread += K
        if i in people:
            if bread <= 0:
                return 'Impossible'
            else:
                bread -= 1
    return 'Possible'

for T in range(int(input())):
    N, M, K = map(int, input().split())
    people = list(map(int, input().split()))
    bread = 0


    print(f'#{T+1} {check()}')
    
# 특정 조건인 경우 더 이상 확인할 필요가 없기 때문에 즉시 종료가 가능한 def 함수를
# 선언해서 구현했고, 조건중 0초일때 붕어빵 판매가 가능한지 생각 후 해결되었다.
```

​	

### D3_4047_영준이의 카드 카운팅

```python
for T in range(int(input())):
    counts = {'S': 13, 'D': 13, 'H': 13, 'C' : 13}

    arr = input()
    new_arr = []
    
    for i in range(0, len(arr), 3): 
        new_arr.append(arr[i:i+3])

    if len(set(new_arr)) != len(new_arr): 
        print(f"#{T+1} ERROR")
    else:
        for i in new_arr: 
            counts[i[0]] -= 1
        print(f"#{T+1} ", end='')
        print(*counts.values())
        
        
# 엄청 좋은 문제라고 생각한다. 막 어렵진 않으나 다양한 스킬들을 사용해서 
# 풀어볼 수 있는 좋은 문제라고 생각한다. 
# 1. set{}의 활용 / 2. for문 slicing / 3. print안에 end 활용 / 4. set자료 접근 등 
```

​	

# SW Expert Academy_D3 5948, 5789, 5688, 5549


​	

### D3_5948_새샘이의 7-3-5 게임

```python
for T in range(int(input())):
    N = list(map(int, input().split()))

    result = set()
    for i in range(5):
        for j in range(i+1, 6):
            for x in range(j+1, 7):
                result.add(N[i]+N[j]+N[x])

    result = list(result)
    result.sort()
    print('#{} {}'.format(T+1, result[-5]))
    
# 3수의 합을 정리하는데 set으로 중복을 제거하는 방법으로 result set을 정리
# set은 순서가 없기 때문에 result를 다시 list로 정리하고 .sort를 이용해 순서대로 나열 후 출력
```

​	

### D3_5789_현주의 상자 바꾸기

```python
for T in range(int(input())):
    N, Q = map(int, input().split())

    arr = ['0' for _ in range(N)]

    for i in range(1, Q+1):
        L, R = map(int, input().split())
        for _ in range(L-1, R):
            arr[_] = str(i)

    print('#{} {}'.format(T+1, ' '.join(arr)))
    
# arr이라는 0이 적힌 상자 리스트를 생성
# index L~R에 _의 숫자를 입력(출력하기 쉽게 str형태로 입력)
# join을 써서 str형태의 list인자를 붙여서 출력
```

​	

### D3_5688_세제곱근을 찾아라

```python
for T in range(int(input())):
    N = round(int(input())**(1/3), 2)

    print("#{} {}".format(T+1, int(N) if int(N) == N else -1))
    
# 출력 방식에 조건을 넣어서 시도해봄
```

​	

### D3_5549_홀수일까 짝수일까 

```python
for T in range(int(input())):
    N = int(input())
    print('#{} {}'.format(T+1, 'Even' if N%2 == 0 else 'Odd'))
    
# 출력 방식에 조건을 넣어서 시도해봄
```



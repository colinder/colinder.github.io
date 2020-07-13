# SW Expert Academy_D3 4676, 4615, 4579, 4522


​	

### D3_4676_늘어지는 소리 만들기

```python
for T in range(int(input())):
    sent = list(input())          # wow
    H = int(input())
    po = sorted(list(map(int, input().split())))        # 2 3 2

    for i in range(H):
        a = po[i]+i
        sent.insert(a,'-')

    print('#{} {}'.format(T+1, ''.join(sent)))
    
# 단순 산수로 해결
```

​	

### D3_4615_재미있는 오셀로 게임

```python

```

​	

### D3_4579_세상의 모든 팰린드롬 2

```python
for T in range(int(input())):
    arr = input()
    result = 'Exist'
    for i in range(len(arr) // 2):
        if arr[i] == '*' or arr[-1 - i] == '*':
            result = 'Exist'
            break
        if arr[i] != arr[-1 - i]:
            result = 'Not exist'
            break

    print(f'#{T+1} {result}')
    
# *이 등장하기 전까지 대칭을 이루고 있다면 무조건 'Exist'이고, 만약 *가 없다면, 대칭인지 판별하면 된다.
```

​	

### D3_4522_세상의 모든 팰린드롬

```python
for T in range(int(input())):
    arr = list(map(str, input()))
    result = 'Exist'
    for i in range(len(arr)):
        if arr[i] == '?':
            arr[i] = arr[-i - 1]
    if arr != arr[::-1]:
        result = 'Not exist'


    print(f'#{T+1} {result}')
    
# ?가 등장하면 무조건 해당 index의 반대쪽이 대칭을 이룬다고 생각할 수 있으니까, ?의 index 반대쪽을 동일하게 변경해주고 대칭인지 판별하면 된다.
```

​	

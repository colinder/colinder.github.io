# SW_20.07.29


​	

### D3_4466_최대 성적표 만들기

```python
for T in range(int(input())):
    N, K = map(int, input().split())
    score = list(map(int, input().split()))

    score.sort(reverse=True)

    result = 0
    for i in range(K):
        result += score[i]

    print('#{} {}'.format(T+1, result))
```

​	

### D3_4406_모음이 보이지 않는 사람

```python
for T in range(int(input())):
    sent = input()
    result = ''
    for i in sent:
        if i not in 'aeiou':
            result += i
    print('#{} {}'.format(T+1, result))
```

​	

### D3_4371_항구에 들어오는 배

```python

```

​	

### D3_4299_태혁이의 사랑은 타이밍

```python
for T in range(int(input())):
    D, H, M = map(int, input().split())
    result = D*1440 + H*60 + M
    if result >= 16511:
        print(f'#{T+1} {result-16511}')
    else:
        print(f'#{T+1} -1')
```

​	

# SW Expert Academy_D3 5431, 5356, 5293, 5215


​	

### D3_5431_민석이의 과제 체크하기

```python
for T in range(int(input())):
    N, K = map(int, input().split())
    P = list(map(int, input().split()))

    result = []
    for i in range(1,N+1):
        if i not in P:
            result.append(str(i))

    print('#{} {}'.format(T+1, ' '.join(result)))
    
# 제출한 수강생 리스트를 만들고(P) 이를 for로 돌리며 검증
```

​	

### D3_5356_의석이의 세로로 말해요

```python
for T in range(int(input())):
    sent = [list(str(input())) for _ in range(5)]


    #입력받은 요소의 길이를 알아내야 한다.
    max = 0
    for n in range(5):
        long = len(sent[n])
        if long >= max:
            max = long

    result = ''
    for i in range(max):
        for j in range(5):
            try:
                if sent[j][i] != "":
                    result += sent[j][i]
            except IndexError:
                pass

    print(f'다시 풀이 #{T+1} {result}')
    
# 입력받은 5개의 단어 중 가자 긴 단어의 길이를 저장하고(long)
# long 길이 만큼 순회를 하면서 5개의 단어에 index로 접근하고 index값이 비어있지 않으면 result에 추가
```

​	

### D3_5293_이진 문자열 복원

```python
for T in range(int(input())):
    A, B, C, D = map(int, input().split())
    if B == 0 and C == 0 and A != 0 and D != 0:
        result = 'impossible'
    elif abs(B - C) > 1:
        result = 'impossible'
    else:
        if B == 0 and C == 0:
            if A != 0:
                result = '0' * (A + 1)
            else:
                result = '1' * (D + 1)
        elif B < C:
            result = '1' * D + '10' * C + '0' * A
        elif B > C:
            result = '0' * A + '01' * B + '1' * D
        else:
            result = '0' * A + '01' * B + '1' * D + '0'
    print('#{} {}'.format(T+1, result))
    
# 하드코딩...ㅎㅎ
```

​	

### D3_5215_햄버거 다이어트

```python
for T in range(int(input())):
    N, maxKcal = map(int, input().split())
    Sum = [0] * (10**4 + 1)

    for _ in range(N):
        Point, Kcal = map(int, input().split())

        for idx in range(maxKcal, Kcal + 1, -1):
            if Sum[idx] < Sum[idx - Kcal] + Point:
                Sum[idx] = Sum[idx - Kcal] + Point

    print(f'#{T+1} {Sum[maxKcal]}')
```



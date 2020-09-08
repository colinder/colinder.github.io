# SW Expert Academy_D3 4698, 1493, , 


​	

### D3_4698_테네스의 특별한 소수

```python
for T in range(int(input())):
    D, A, B = map(int, input().split())
    D = str(D)
    a = [0, 0] + [1] * (B - 1)
    rootB = int(B**0.5)							👈이 아이디어를 얻기까지 오랜시간이 걸렸다.

    for i in range(2, rootB+1):					👈
        if a[i] == 1:							👈
            for j in range(2*i, B+1, i):		👈
                a[j] = 0						👈

    result = []
    for i in range(A, B+1):
        if a[i] == 1 and D in str(i):
            result.append(i)

    print(f'#{T+1} {len(result)}')
```

​	

### D3_1493_수의 새로운 연산

```python
new_number = [(0, 0)]
count = 1
while count <= 300:
    count += 1
    for x in range(1, count):
        y = count - x
        new_number.append((x, y))

for T in range(int(input())):
    p, q = map(int, input().split())
    x = new_number[p][0] + new_number[q][0]
    y = new_number[p][1] + new_number[q][1]
    result = new_number.index((x, y))
    print(f'#{T+1} {result}')

# 별다른 아이디어는 없었다. 그냥 인덱스별 좌표를 미리 만들어 두고 
# 그걸 불러오는 방법을 생각했다. (런타임이 겁났지만, Pass 했다.)
```

​	

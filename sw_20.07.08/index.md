# SW Expert Academy_D3 , , , 


​	

### D3_5162_두가지 빵의 딜레마

```python
for T in range(int(input())):
    A, B, C = map(int, input().split())

    N = min(A, B)
    print("#{} {}".format(T+1, int(C/N)))
    
# 단순 산수로 해결
```

​	

### D3_4789_성공적인 공연 기획

```python
for T in range(int(input())):
    P = list(map(int, map(str, input())))	
    need = P[0]
    count = 0
    for i in range(1, len(P)):
        if need >= i:
            need += P[i]
        else:
            count += i - need
            need = i + P[i]

    print(f'#{T+1} {count}')
    
# 문제를 이해하는 것이 Point였다.
# 먼저 str로 input을 받아 개별 숫자로 나누고 바로 int 변경하여 인자를 받음.
# 
# "i번째 글자가 의미하는 바는 기립 박수를 하고 있는 사람이 i-1명 이상일 때 기립 박수를 하는 사람의 수"
# Test case3의 경우 (09) 9는 큰 의미 없이 2번째 글자임으로 기립박수를 하고 있는 사람이 1명일 이상일 때 기립박수를 친다.
# 즉 index만 가지고 조건을 판단 하면 된다!
```

​	

### D3_4751_다솔이의 다이아몬드 장식

```python
T= int(input())

for tc in range(T):
    text = input()
    n= len(text)
    for i in range(5):
        if i == 0 :
            print('..#.'*n+'.')
        elif i == 1 :
            print('.#'*(n*2)+'.')
        elif i == 2 :
            print('#',end='')
            for a in range(len(text)) :
                print(('.{}.#'.format(text[a])), end="")
        elif i == 3 :
            print()
            print('.#'*(n*2)+'.')
        elif i == 4 :
            print('..#.' * n + '.')
            
# 단순 작업이었으나, 반복되는 곳의 기준을 어디로 할 것인가를 잘 지정해야 함.
```

​	

### D3_4698_테네스의 특별한 소수

```python

```



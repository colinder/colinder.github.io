# SW Expert Academy_D3 11445, 11387


​		

### D3_11445_무한 사전

```python
for T in range(int(input())): 
    P = input().rstrip()
    Q = input().rstrip()

    if P + "a" != Q:
        result = "Y"
    else:
        result = "N"

    print(f'#{T+1} {result}')
    
# 왜 D3 인가.
```

​	

### D3_11387_몬스터 사냥

```python
for T in range(int(input())):
    D, L, N = map(int, input().split())
    
    result = 0
    for i in range(N):
        result += D * (1 + L*i/100)
    
    print(f'#{T+1} {int(result)}')
```

​	

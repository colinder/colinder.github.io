# SW Expert Academy_D4 6959, , , 


​	

### D4_6959_이상한 나라의 덧셈게임

```python
for T in range(int(input())):
    N = input()
    result = ["A", "B"]
    turn = 1
    while len(N) > 1:
        N = str(int(N[0]) + int(N[1])) + N[2:]
        turn += 1
    print(f'#{T+1} {result[turn%2]}')
    
# 단순 산수
```

​	

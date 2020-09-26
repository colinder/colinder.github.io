# SW Expert Academy_D3 3975, , , 


​	

### D3_3975_승률 비교하기

```python
result = []
for T in range(int(input())):
    A, B, C, D = map(int, input().split())
    if B/A > D/C:
        result.append("BOB")
    elif B/A < D/C:
        result.append("ALICE")
    else:
        result.append("DRAW")

for t in range(T+1):
    print(f"#{t+1} {result[t]}")
    
# 아니 이게 D3라고? 완전 난이도 설정 실수네. 했다가 런타임 오류 보고
# 왜 그런건지 알았다.
# python은 결과를 모았다가 출력하는 게 더 빠르다.
```

​	

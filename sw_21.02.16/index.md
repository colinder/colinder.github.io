# SW Expert Academy_D4 3143


​	

### D4_3143_가장 빠른 문자열 타이핑

```python
for T in range(int(input())):
    A, B = input().split()
    result = len(A) - (A.count(B)*(len(B)-1))
            
    print(f'#{T+1} {result}')
    
# 왜 D4 인가.
```

​	



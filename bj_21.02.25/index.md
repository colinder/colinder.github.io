# BEAKJOON 5052


​	

### 5052_전화번호 목록

```python
import sys

for T in range(int(sys.stdin.readline().rstrip())):
    N = int(sys.stdin.readline().rstrip())

    Numbers = []
    for _ in range(N):
        Numbers.append(sys.stdin.readline().rstrip())

    Numbers.sort()

    result = "YES"
    for i in range(len(Numbers)-1):
        if Numbers[i+1].find(Numbers[i], 0, len(Numbers[i])) != -1:
            result = 'NO'
            break

    print(result)

    
# 흠.. 다른 사람들은 트리로 풀었나..?
# 이게 왜 트리에 있지...
```

​	

# BEAKJOON 1918, , , 


​	

### 1918_후위 표기식

```python
import sys
from collections import deque

text = sys.stdin.readline().rstrip()

# 후위 표기법 알고리즘
operator = {"+":1, "-":1, "*":2 , "/":2, "(":0, ")":0}
stack = deque()
result = ""

for i in text:
    if i not in operator:
        result += i
    elif i == "(":
        stack.append(i)
    elif i == ")":
        while stack and stack[-1] != "(":
            result += stack.pop()
        stack.pop()
    else:
        while stack and operator[i] <= operator[stack[-1]]:
            result += stack.pop()
        stack.append(i)

while stack:
    result += stack.pop()
                
print(result)
```

​	

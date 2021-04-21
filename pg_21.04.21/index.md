# programmers_21.04.21


​	

### 타겟 넘버

```python
numbers = [1, 1, 1, 1, 1]
target = 3
# return = 5

from itertools import product

l = [(-number, number)for number in numbers]
s = list(map(sum, product(*l)))
print(s.count(target))


## 미쳤다. product()..
```

​	

### K번째 수

```python
array = [1, 5, 2, 6, 3, 7, 4]	
commands = [[2, 5, 3], [4, 4, 1], [1, 7, 3]]


def solution(array, commands):
    answer = []
    for _ in commands:
        i, j, k = _
        q = array[i-1: j]
        q.sort()
        answer.append(q[k-1])
    return answer

print(solution(array, commands))
```

​	

### 다리를 지나는 트럭

```python
bridge_length	weight	truck_weights	                    return
2	            10	    [7,4,5,6]  	                            8
100	            100	    [10]	                              101
100	            100	    [10,10,10,10,10,10,10,10,10,10]       110


from collections import deque

truck_weights = deque(truck_weights)
bridge = deque([0 for _ in range(bridge_length)])
time = 0
bridge_weight = 0   #현재 다리를 건너고 있는 무게

# bridge = [0, 0]
while(len(bridge)) != 0
    out = bridge.popleft()
    bridge_weight -= out
    time += 1
    if truck_weights:
        if bridge_weight + truck_weights[0] <= weight:
            left = truck_weights.popleft()
            bridge_weight += left
            bridge.append(left)
        else:
            bridge.append(0)
            

## 다리가 지나는 위치를 bridge로 표현하고 디큐를 이용해 속도를 올린다.
```





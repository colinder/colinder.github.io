# BEAKJOON 1149


​	

### 1149_RGB거리

```python
N = int(input())
RGB = [list(map(int, input().split())) for _ in range(N)]

for i in range(1, len(RGB)):
    RGB[i][0] = RGB[i][0] + min(RGB[i-1][1], RGB[i-1][2])
    RGB[i][1] = RGB[i][1] + min(RGB[i-1][0], RGB[i-1][2])
    RGB[i][2] = RGB[i][2] + min(RGB[i-1][0], RGB[i-1][1])
print(min(RGB[i][0], RGB[i][1], RGB[i][2]))

# 2시간이 넘게 고민한 결과...
# 문제 속 조건3 ""i(2 ≤ i ≤ N-1)번 집의 색은 i-1번, i+1번 집의 색과 같지 않아야 한다.""을 이해하는데 시간이 걸렸다.
# 조건3에 따르면 N=3인 경우, i = 2가 되고, 결국 1번, 3번 집의 색과 2번 집의 색이 다르면 된다.
# 난 이걸.. 1번집과 3번집의 색이 달라야 한다고 잘못 이해해서 시간을 버렸다.ㅎ
```

​	

### 1932_정수삼각형

```python
N = int(input())
tree = [list(map(int, input().split())) for _ in range(N)]

for i in range(1, len(tree)):
    f = len(tree[i])
    for j in range(f):
        if j == 0:
            tree[i][j] = tree[i][j] + tree[i-1][j]
        elif j == f-1:
            tree[i][j] = tree[i][j] + tree[i-1][j-1]
        else:
            tree[i][j] = tree[i][j] + max(tree[i-1][j-1], tree[i-1][j])
print(max(tree[i]))

# 이전 문제를 풀고선 그리 어렵지않게 풀었다. 
```

​		

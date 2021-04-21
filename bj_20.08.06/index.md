# BEAKJOON 1149, 1932, 2579, 1463


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

### 2579_계단오르기

```python
# 1. 계단은 한 번에 한 계단씩 또는 두 계단씩 오를 수 있다. 즉, 한 계단을 밟으면서 이어서 다음 계단이나, 다음 다음 계단으로 오를 수 있다.
# 2. 연속된 세 개의 계단을 모두 밟아서는 안 된다. 단, 시작점은 계단에 포함되지 않는다.
# 3. 마지막 도착 계단은 반드시 밟아야 한다.

N = int(input())

# stairs = [int(input()) for _ in range(N)]
# 이렇게 정보를 받으면 느리다....
stairs = [0 for _ in range(301)]
for i in range(N):
    stairs[i] = int(input())

total = [0 for _ in range(301)]

total[0] = stairs[0]
total[1] = stairs[0] + stairs[1]
total[2] = max(stairs[1] + stairs[2], stairs[0] + stairs[2])
# 마지막 계단을 반드시 밟아야 하니까. <0: 안밟음, 1: 밟음>
for i in range(3, N):
    total[i] = max(
        total[i-3]+stairs[i-1]+stairs[i], # 1 0 1 1(마지막계단)의 경우
        total[i-2]+stairs[i]              #   1 0 1(마지막계단)의 경우
        )

print(total[N-1])
```

​		

### 1463_1로 만들기

```python
x = int(input())
x_map = [0 for i in range(x+1)]
visited = [0 for i in range(10**6)]

x_map[x] = 1
count = 0
while x_map[1] == 0:
    count += 1
    for i in range(1, x+1):
        if x_map[i] == count:
            if i % 3 == 0 and visited[i//3] == 0:
                x_map[i//3] = count+1
                visited[i//3] = 1
            if i % 2 == 0 and visited[i//2] == 0:
                x_map[i//2] = count+1
                visited[i//2] = 1
            if x_map[i-1] == 0 and visited[i-1] == 0:
                x_map[i-1] = count+1
                visited[i-1] = 1

print(x_map[1]-1)

# 코딩하면서는 너무 바보 같은 방법인가.. 했지만, 통과됬으니 뭐.. 만족한다.
```

​		

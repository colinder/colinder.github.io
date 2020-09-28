# SW Expert Academy_D3 3975, 5176, , 


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

### [D2_5176_이진탐색](https://github.com/colinder/colinder.github.io/blob/master/images/binary_tree.png)

```python
def makeTree(n):
    global count
    #Tree 범위 제한
    if n <= N:
        #1부터 시작한다고 가정했을 때 왼쪽노드는 현재 인덱스의 2배
        makeTree(n*2)
        #더이상 못가면 값넣기
        tree[n] = count
        #값 넣었으면 증가시키기
        count += 1
        #우측 노드는 인덱스 2배 + 1
        makeTree(n*2 + 1)

for T in range(int(input())):
    N = int(input())
    tree = [0 for i in range(N+1)]
    count = 1
    makeTree(1)
    
    print(f'#{T+1} {tree[1]} {tree[N//2]}')

# 2020하반기 네이버 코테를 풀고 tree에 대하여 깊이있게 공부해봐야겠다고 생각하며
# 다시 처음부터 공부하는데, 미쳤다.ㅎ
```



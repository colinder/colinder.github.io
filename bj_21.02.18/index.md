# BEAKJOON 1991, 11725, 1967, 1167


​	

### 1991_트리 순회

```python
class Node:
    def __init__(self, data, L, R):
        self.data = data
        self.left = L
        self.right = R

def preorder(node):     # 전위 순회
    print(node.data, end="")
    if node.left != ".":
        preorder(tree[node.left])
    if node.right != ".":
        preorder(tree[node.right])

def inorder(node):      # 중위 순회
    if node.left != ".":
        inorder(tree[node.left])
    print(node.data, end="")
    if node.right != ".":
        inorder(tree[node.right])

def postorder(node):      # 후위 순회
    if node.left != ".":
        postorder(tree[node.left])
    if node.right != ".":
        postorder(tree[node.right])
    print(node.data, end="")

N = int(input())

tree = {}
for _ in range(N) :
    data, L, R = input().split()
    tree[data] = Node(data, L, R)


preorder(tree['A'])
print()
inorder(tree['A'])
print()
postorder(tree['A'])

# 트리 구조에 대한 이해가 부족하다.
# 트리문제를 많이 풀어봐야 겠다.
# 해보니 복잡?하지는 않은데 익숙하지 않은 느낌이다.
```

​	

### 11725_트리의 부모 찾기

```python
import sys
from collections import deque

N = int(sys.stdin.readline().rstrip())
    
tree = [[] for _ in range(N+1)]

for _ in range(N-1):
    S, E = map(int, sys.stdin.readline().split())
    tree[S].append(E)
    tree[E].append(S)

q = deque([1])
visited = [True, True] + [False for _ in range(N)]
result = [[] for _ in range(N+1)]
while len(q):
    parent = q.popleft()
    for child in tree[parent]:
        if not visited[child]:
            visited[child] = True
            result[child].append(parent)
            q.append(child)

for j in range(2, len(result)):
    print(result[j][0])

# heap을 사용하는 건가 고민했으나, 아니었고,
# 1이 root node인 것을 알고 있으니.
# 각각의 node에 연결되어있는 모든 node들을 기록했다가.
# 1부터 BFS로 한 단계씩 내려가면, 코딩상 pop한 것이 
# 부모 node가 된다.

# 채점 속도가 느려서 런타임 오류 걸릴까봐 조마조마했다.
```

​	

### 1967_트리의 지름

```python
import sys
from collections import deque

def BFS(value, step):
    q = deque()
    q.append(value)
    visited = [-1 for _ in range(N+1)]
    visited[value] = 0
    while q:
        x = q.popleft()
        for c, w in tree[x]:
            if visited[c] == -1:
                visited[c] = visited[x] + w
                q.append(c)
    if step == 1:
        return visited.index(max(visited))
    else:
        return max(visited)

N = int(sys.stdin.readline().rstrip())
    
tree = [[] for _ in range(N+1)]

for i in range(N-1):
    P, C, V = map(int, input().split())
    tree[P].append([C, V])
    tree[C].append([P, V])

print(BFS(BFS(1, 1), 2))


# DFS로 지저분하게 구현했으나 말 그대로
# 너무 지저분해서 검색의 도움을 받음.

# root인 1에서 가장 멀리 있는 노드를 고르고
# 그 노드에서 가장 멀리 있는 노드를 고르는 
# 알고리즘으로 구현. 
```

​	

### 1167_트리의 지름

```python
import sys
from collections import deque

sys.setrecursionlimit(10**9)

def BFS(i, mode):
    q = deque()
    q.append(i)
    visited = [-1 for _ in range(N+1)]
    visited[i] = 0
    while q:
        x = q.popleft()
        for C, W in tree[x]:
            if visited[C] == -1:
                visited[C] = visited[x] + W
                q.append(C)
    if mode == 1:
        return visited.index(max(visited))
    else:
        return max(visited)


N = int(sys.stdin.readline().rstrip())

tree = [[] for _ in range(N+1)]

for _ in range(N):
    info = list(map(int, sys.stdin.readline().split()))
    for i in range(1, len(info)-1, 2):
        tree[info[0]].append([info[i], info[i+1]])
    
print(BFS(BFS(i, 1), 2))
```

​	

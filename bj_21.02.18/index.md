# BEAKJOON 1991, 


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

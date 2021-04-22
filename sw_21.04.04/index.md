# SW Expert Academy_D4 11316


​	

### D4_11316_주기 찾기

```python
for T in range(int(input())):
    s, p, q, m = map(int, input().split())
    V = s
    visited = [0]*m
    i = 1
    while True:
        V = (p*V + q) % m
        i += 1
        if visited[V] != 0:
            result = i - visited[V]
            break
        visited[V] = i

    print(f'#{T+1} {result}')

# 한 번 나왔던 숫자의 '위치 정보(i)'를 
# visited에 저장해 두었다가
# 한 번이라도 방문했던 숫자가 나왔을 때
# 저장해주었던 '위치 정보'의 차리를 계산해 준다.

# 코딩중. 
# 만약 [6,8,6,4,3,5,4,3,5,4,3,5...] 경우가 있을 수 있나? 는 고민으로 
# 시간을 썼으나. 
# 문제 조건 중
# 슈도랜덤 제너레이터의 주기란,
# 어떤 정수 n0 이상인 '모든 n에 대해' An+p = An을 만족하는 가장 작은 자연수 p
# 라는 내용을 보고, 반복되는 수의 등장은 동일 배열 순환의 시작이라고 파악해야 했다.
```

​	

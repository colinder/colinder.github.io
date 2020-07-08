# SW Expert Academy_D3 9700, 7675, 6692, 6485


​	

### D3_9700_USB 꽂기의 미스터리

```python
for T in range(int(input())):
    p, q = map(float, input().split())
    s1 = (1-p)*q
    s2 = p*(1-q)*q

    if s1 < s2:
        print(f'#{T+1} YES')
    else:
        print(f'#{T+1} NO')
        
# 단순 수학을 통해 해결.
```

​	

### D3_7675_통역사 성경이

```python
for T in range(int(input())):
    N = int(input())
    sent = input()
    sent = sent.replace('.', ' @').replace('!', ' @').replace('?', ' @').split()
    #print(sent)
    result = ""
    count = 0
    for i in sent:
        if i.isalpha() and i == i.capitalize():
            count += 1
        if i == '@':
            result += str(count) + ' '
            count = 0
    print("#{} {}".format(T+1, result))

# 주어지는 문장을 띄어쓰기를 기준으로 나누기 위해 .split()을 사용하고 
# 문제에서 주어진 `이름`의 조건이 `첫 알파벳이 대문자이고 나머진 소문자`이기 때문에
# 문자로만 이루어져있는 것인지 .isalpha()로 검증 (VS .isdigit() 주어진 문자열이 숫자인지 검증)
# `이름`의 조건이 맞는지 .capitalize()로 검증
	# 추가 공부 내용
	# upper - 주어진 문자열에서 모든 알파벳들을 대문자로 변환시킨다.
    # capitalize - 주어진 문자열에서 맨 첫 글자를 대문자로 변환하고 나머지는 소문자로 변환시킨다.
    # title	- 주어진 문자열에서 알파벳 외의 문자(숫자, 특수기호, 띄어쓰기 등)로 나누어져 있는 영단어들의 첫 글자를 모두 대문자로 변환시킨다.

# .isalpha()와 .capitalize()를 알고나면 간단해지는 문제.
```

​	 	

### D3_6692_다솔이의 월급 상자

```python
for T in range(int(input())):
    tc = int(input())

    result = 0
    for _ in range(tc):
        P, X = map(float, input().split())
        result += P*X

    print(f'#{T+1} {result}')
    
# input을 float형으로 받으면서 단순 수학을 사용해 해결
```

​	

### D3_6485_삼성시의 버스 노선

```python
for T in range(int(input())):
    info = [0]*5001
    for N in range(int(input())):
        st, la = map(int, input().split())
        for x in range(st, la+1):
            info[x] +=1

    station = []
    for P in range(int(input())):
        station.append(str(info[int(input())]))

    print('#{} {}'.format(T+1, ' '.join(station)))
    
# info로 정류장의 index를 기록(중복되는 위치를 표기하기 위해 +1 씩 진행)
# P개의 버스 정류장을 확인하는데 저장되어있는 info의 정류장 정보를 가져와서 station리스트에 기록

# 주어진 문제에서 만약 범위?를 알려준다면 그만큼의 저장 리스트를 생성하는 것이 빠르다.
# 예를 들어 info = [] 보다 info = [0]*5001 로 설정해 사용하는 것이 더 빠른 실행시간을 보여준다.
```

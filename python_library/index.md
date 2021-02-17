# Python_Library


​	

# Python Library

- ### **raise** **예외('에러메시지')**

  ```python
  # python에서는 의도적으로 오류를 일으킬 수 있는데
  # 이를 해주는 것이 raise 메서드.
  
  try:
      x = 2
      if x % 3 != 0:                               
          raise Exception('3의 배수가 아닙니다.')  
      print("입력된 값", x)
  except Exception as e:                      
      print('예외가 발생했습니다.', e)
      
  >>> 예외가 발생했습니다. 3의 배수가 아닙니다.
  ```
  
  ​	
  
- ### filter() VS find()

  ```python
  ## filter(function, iterable)
  # 배열의 `모든 요소`에 접근하여 조건에 맞는 `값`을 찾는다.
  
  # ex)
  def func(x):
      if x > 0:
        return x
      else:
        return None
  
  list(filter(func, range(-5,10)))
  >>> [1, 2, 3, 4, 5, 6, 7, 8, 9]
  
  # ex)
  [ i for i in range(-5,10) if i > 0 ]
  >>> [1, 2, 3, 4, 5, 6, 7, 8, 9]
  
  
  # find(sub[, start[, end]])
  # 배열을 순회하면서 찾으려 하는 값의 `index`를 반환 해준다.
  
  # ex)
  a = 'hello'
  a.find('o')  # find 함수
  >>> 4
  ```

  ​	

- ### index() VS find()

  ```python
  ## 공통점
  # 두 함수 모두 찾으려 하는 값의 `index`를 반환 해준다.
  # ex)
  >>> 'oxoxoxoxox'.find('x')  # find 함수
  1 
  >>> 'oxoxoxoxox'.index('x')  # index 함수
  1
  
  # 문자 'o'가 첫번째 위치한 자리를 출력
  >>> a = 'hello'
  >>> a.find('o')  # find 함수
  4
  >>> a.index('o')  # index 함수
  4
  
  # 또 (value, start, end) 형태로 문자를 찾는 시작점과 종료점을 지정할 수 있다.
  # 문자열중 2번째 위치부터 처음 'x'가 위치한 자리
  >>> 'oxoxoxoxox'.index('x', 2)
  3
  
  # a변수에서 1번째~3번째 사이에 문자 'o'가 위치한 자리
  >>> a = 'hello'
  >>> a.find('o', 1, 3)
  -1	
  # find함수는 찾는 값이 없을 때 -1을 출력한다. 이게 차이점.
  
  
  ## 차이점
  # 찾으려는 값이 없는 경우 find()는 '-1'을 반환 / index()는 'ValueError' 반환
  ```

  ​	

- ### deque

  ```python
  # deque는 기본적으로 리스트를 변형(ex. pop() ...)하며 코딩할 때
  # 속도가 빠르기 때문에 사용한다. 
  # 주로 popleft()를 사용하지만, 유용한 기능이 더 있다. 
  
  ## 1. rotate
  from collections import deque
  
  q = deque([1,2,3,4,5,6])
  q.rotate(3)
  print(q)	# deque([4, 5, 6, 1, 2, 3])
  
  # 주로 리스트의 끝이 붙어 있는 원순열 문제를 해결하는데 편리하다. 
  # 다만 속도가 어떤지는 잘 모르겠다. 
  
  ## 2. deque는 슬라이싱이 되지 않는다.
  # 굳이 해야 한다면 아래를 참고
  deque_slice = collections.deque(itertools.islice(my_deque, 10, 20))
  
  # deque는 
  # index()는 '사용가능'하고 find()는 '사용불가'하다. 
  ```

  ​	

- ### q[len(q)-1] VS q[-1] 속도 비교

  ```python
  import time
  start = time.time()  # 시작 시간 저장
  ...
  ...
  print("time :", time.time() - start)  # 현재시각 - 시작시간 = 실행 시간
  
  # q[len(q)-1] VS q[-1] 속도 비교
  # q[len(q)-1] 이 더 빠름.
  # 왜..?
  ```

  ​	

- ### isalpha(), isdigit()

  ```python
  # 알파벳이냐?, 숫자형이냐? 를 참, 거짓 형태로 반환
  ```

  ​	

- ### sys.setrecursionlimit(최대 재귀 깊이 설정)

  ```python
  # 최대 재귀 깊이를 늘리려면 sys 모듈의 setrecursionlimit 함수를 사용
  # (기본값이상으로 안해주면 런타임에러로 처리된다.) ※기본값:1000 
  sys.setrecursionlimit(50000)
  ```

  ​	

- ### sorted()

  ```python
  sorted는 여러 그룹의 값이 주어진 경우
  
  ## 순차적으로 증감을 비교해 준다.
  # ex)
  A = [(1,2,1), (1,2,2,3), (1,1,3), (1,1,2)]
  A = sorted(A)
  print(A)    # [(1, 1, 2), (1, 1, 3), (1, 2, 1), (1, 2, 2, 3)]
  
  ## 만약 주어진 두번째 인자들을 기준으로 정렬하고 싶다면?
  A = [(0, 4), (2, 2), (1, 2), (1, -1), (3, 3)]
  B = sorted(A, key=lambda L: L[1])
  print(B)	# [(1, -1), (2, 2), (1, 2), (3, 3), (0, 4)]
  
  ## 두번째 인자 우선 기준 후 첫번째 인자 기준 정렬하고 싶다면?
  A = [(0, 4), (2, 2), (1, 2), (1, -1), (3, 3)]
  B = sorted(A, key=lambda L: (L[1], L[0]))
  print(B) 	# [(1, -1), (1, 2), (2, 2), (3, 3), (0, 4)]
  
  ## set자료형을 sorted 하면 자동으로 list type으로 변환된다.
  a = {"a","b","C"}
  print("a의 타입: ",type(a))	 # a의 타입:  <class 'set'>
  b = sorted(a)
  print("b의 타입: ", type(b))	 # b의 타입:  <class 'list'>
  ```

  ​	

- ### <span style="color: #7500ab">from</span> collections <span style="color:#7500ab">import</span> Counter

  주어지는 컨테이너<span style="font-size:.8em">(ex 리스트 자료형, string)</span>에 동일한 값의 갯수를 파악

  ```python
  from collections import Counter
  MoResult = Counter([7, 1, 2, 5, 1, 8, 7, 6]).most_common()
  print(MoResult)		#[(7, 2), (1, 2), (2, 1), (5, 1), (8, 1), (6, 1)]
  
  # .most_common() 함수
  # 입력된 인자들의 '순서'를 존중하면서, '중복 count해줌'과 동시에 '중복 삭제'까지 진행.
  ```

  ​	

- ### <span style="color: #7500ab">import</span> sys

  빠른 속도로 입력받기 위한 라이브러리

  ```python
  imput VS sys.stdin.readline()
  
  # python의 대표적인 입력 함수
  imput()
  
  
  # 단, 백준에는 더 빠른 방법이 초기에 설명되어있다.
  import sys
  sys.stdin.readline()	
  # 단 위와 같이 입력 받은 경우, 마지막에 개행 문자까지 입력되어 뒤에
  # 추가 함수(rstrip())를 붙여 사용하는 것이 보통이다. 
  ```

  ​		

- ### 정규표현식

  >- 일대일 매칭되는 문자
  >
  >>정규표현식 안에서, 바로 다음 절에서 설명하는 메타문자를 제외한 모든 문자 하나는 일반 문자열 하나와 매칭된다. 예를 들어, `a`는 a와 매칭되고, `가`는 ‘가’와 매칭되는 식이다.
  >>당연히 `a`가 ‘b’ 또는 ‘가’와 매칭되지는 않는다.
  >
  >>어떤 프로그래밍 언어의 정규표현식이든 메타문자(특수한 기능을 하는 문자)라는 것이 존재한다.
  >>
  >>파이썬 re 모듈의 메타문자는 총 12개로 다음과 같은 것들이 있다.
  >>
  >>$()*+.?[\^{|
  >
  >>이들 메타문자는 각각의 문자 하나에 매칭되지 않는다.
  >>예를 들어 일반 문자인 `a`는 문자 `‘a’`에 매칭하지만, 
  >>여는 소괄호 `(`는 문자 `‘(‘`와 매칭하지 않는다.
  >
  >>그럼 찾고자 하는 문자열에 소괄호가 있으면 어떻게 할까?
  >
  >>위의 문자들의 앞에 백슬래시 `\`를 붙여 주면 일반 문자처럼 한 글자에 매칭된다. 
  >
  >>예를 들어 `\(`는 문자 `‘(‘`와 매칭된다.

    		

- ### find(찾을문자, 찾기시작할위치) - <span style="color:red">찾는 값의 인덱스를 반환</span>

  ```python
  s = '가나다라 마바사아 자차마타 파하'
  print(s.find('마'))				# s에서 첫'마'의 인덱스를 반환
  print(s.find('마', 3))			# s[3:]부터 첫'마'의 인덱스를 반환 (전체 범위 기준)
  print(s.find('가', 2))			# s[2:]부터 첫'가'의 인덱스를 반환 (없으면 -1 반환)
  ```

  ```python
  5
  5
  -1
  ```

  ​	

- ### startsWith(찾을문자, 찾기시작위치, 찾기종료위치)

  ```python
  str = "this is string";
  print(str.startswith('this'))			# str맨앞이 'this'로시작하는지 검사
  print(str.startswith('is', 2, 4))		# str[2:4]에서 맨앞이 'is'인지 검사
  print(str.startswith('this', 3, 4))		# str[3:4]에서 맨앞이 'this'인지 검사
  ```

  ```python
  True
  True
  False
  ```

  ​	

- ## <span style="color:#7500ab">import</span> itertools (효율적인 반복을 위한 함수)

  - ### permutations(자료, 만들순열길이)

    n개의 원소를 사용해서 순서를 정하여 r개의 배열로 나타내는 것 => **순열 공식 : _nPr = n!/(n-r)!_**

    ```python
    import itertools
    
    a = [1,2,3]
    permute = itertools.permutations(a,2)
    print(list(permute))
    
    a = "abc"
    permute = itertools.permutations(a,2)
    print(list(permute))
    ```

    ```python
    [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]
    [('a', 'b'), ('a', 'c'), ('b', 'a'), ('b', 'c'), ('c', 'a'), ('c', 'b')]
    ```

    ​	

  - ### combinations(조합)

    n개의 원소를 사용해서 순서의 관계없이 r개의 배열로 나타내는 것 => **조합 공식 : _nCr=nPr/r!_**

    ```python
    import itertools
    
    a = [1,2,3]
    permute = itertools.combinations(a, 2)
    print(list(permute))
    
    a = "abc"
    permute = itertools.combinations(a, 2)
    print(list(permute))
    ```

    ```python
    [(1, 2), (1, 3), (2, 3)]
    [('a', 'b'), ('a', 'c'), ('b', 'c')]
    ```

    

  

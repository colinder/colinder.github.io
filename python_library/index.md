# Python_Library


​	

# 자주 쓰는 Python Library

> 자주 사용하게되는 파이썬 라이브러리를 정리한다.

​	 

- ### 정규표현식

  >일대일 매칭되는 문자
  >
  >>정규표현식 안에서, 바로 다음 절에서 설명하는 메타문자를 제외한 모든 문자 하나는 일반 문자열 하나와 매칭된다. 예를 들어, `a`는 a와 매칭되고, `가`는 ‘가’와 매칭되는 식이다.
  >>당연히 `a`가 ‘b’ 또는 ‘가’와 매칭되지는 않는다.
  >>
  >>```메타문자
  >>어떤 프로그래밍 언어의 정규표현식이든 메타문자(특수한 기능을 하는 문자)라는 것이 존재한다.
  >>
  >>파이썬 re 모듈의 메타문자는 총 12개로 다음과 같은 것들이 있다.
  >>
  >>$()*+.?[\^{|
  >>
  >>이들 메타문자는 각각의 문자 하나에 매칭되지 않는다.
  >>예를 들어 일반 문자인 `a`는 문자 `‘a’`에 매칭하지만, 
  >>여는 소괄호 `(`는 문자 `‘(‘`와 매칭하지 않는다.
  >>
  >>그럼 찾고자 하는 문자열에 소괄호가 있으면 어떻게 할까?
  >>
  >>위의 문자들의 앞에 백슬래시 \를 붙여 주면 일반 문자처럼 한 글자에 매칭된다. 
>>예를 들어 `\(`는 문자 `‘(‘`와 매칭된다.
  >>```
  
  
  
  ​	
  
- ### find(찾을문자, 찾기시작할위치)

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

- ## itertools (효율적인 반복을 위한 함수)

  - ### permutations(자료, 만들순열길이)

    - n개의 원소를 사용해서 순서를 정하여 r개의 배열로 나타내는 것 => **순열 공식 : _nPr = n!/(n-r)!_**

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

    - n개의 원소를 사용해서 순서의 관계없이 r개의 배열로 나타내는 것 => **조합 공식 : _nCr=nPr/r!_**

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

    

  

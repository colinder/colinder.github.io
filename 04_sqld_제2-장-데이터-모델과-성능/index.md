# 04_SQLD_제1 절 성능데이터모델링의 개요 & 제2 절 정규화와 성능


​		

# # SQLD

---

# 제2 장 데이터 모델과 성능

---

## 제1 절 성능데이터모델링의 개요

1. 성능데이터모델링의 정의

   - 데이터 용량이 커질수록 처리 속도 증가가 필요해진다.
   - 성능 저하의 대표적인 원인 3가지
     1. 데이터 모델 구조에 의해 성능 저하
     2. 데이터가 대용량이 됨으로 인해 성능 저하
     3. 인덱스 특성을 고려하지 않고 생성해 성능 저하

   - **즉, 어떤 작업 유형에 따라 성능향상을 도모해야 하는지 목표를 분명하게 해야 정확한 성능향상 모델링을 할 수 있다.** 

     ​			

2. 성능데이터모델링 수행 시점

   - 사전에 할수록 비용이 줄어들며, 특히 분석/설계단계에서 데이터베이스 처리 성능을 향상시킬 수 있는 방법을 고려하면 좋다.

     ​			

3. 성능데이터모델링 고려사항

   1. 데이터 모델링을 할 때 정규화를 정확하게 수행한다.

   2. 데이터베이스 용량산정을 수행한다.

   3. 데이터베이스에 발생되는 트랜잭션의 유형을 파악한다.

   4. 용량과 트랜잭션의 유형에 따라, 반정규화를 수행한다.

   5. 이력모델의 조정, PK/FK조정, 슈퍼타입/서브타입 조정 등을 수행한다.

   6. 성능관점에서 데이터 모델을 검증한다.

      ​	

---

## 제2 절 정규화와 성능

1. 정규화를 통한 성능 향상 전략

   - **정규화**는 기본적으로 데이터에 대한 중복성을 제거하고, 데이터가 관심사별로 처리되는 경우가 많아 **대부분 성능을 향상**시키는 특징이 있다.

   - 다만, 성능을 판단할 때 <b>(조회)</b>와  **(입력,수정,삭제)** 두 부류로 구분하여 고려한다.

     > "엔터티가 많은 경우" 다량의 JOIN이 발생하고 그런 경우 보통 <b>(조회)</b> 성능은 **하락**하고 / <b>(입력,수정,삭제)</b> 성능은 **향상**된다. 

     ---

   - **1차 정규화** : table의 cell값은 **원자 값**을 가져야한다. <span style="font-size:.9em; color: grey">== 중복된 값을 제거 or 분할. (row, column 모두)</span>

     - 행(row) 정규화

       <center><span style="color:grey; font-size:.8em">1차 정규화 전</span></center>

       | student | age  | subject                                    |
       | ------- | ---- | ------------------------------------------ |
       | 보리    | 1    | 수학, <span style='color: red'>과학</span> |
       | 쌀      | 2    | 수학                                       |
       | 수수    | 3    | 수학                                       |

       <center><span style="color:grey; font-size:.8em">1차 정규화 <span style="color:red">후</span></span></center>

       | student                              | age                               | subject                                      |
       | ------------------------------------ | --------------------------------- | -------------------------------------------- |
       | 보리                                 | 1                                 | 수학<span style='color: white'>, 과학</span> |
       | <span style='color: red'>보리</span> | <span style='color: red'>1</span> | <span style='color: red'>과학</span>         |
       | 쌀                                   | 2                                 | 수학                                         |
       | 수수                                 | 3                                 | 수학                                         |

       ​			

     - 열(column) 정규화

       {{<image src="/images/SQLD_10.png" width="1000px">}}

       ​			

       ​			

   - **2차 정규화** : 테이블의 **부분함수적 종속** 제거 <span style="font-size:.9em; color: grey">== 테이블 값이 **완전 함수적 종속**인지 확인해야 한다.</span>

     <p style="font-size:0.9em; text-align:right; font-style:italic;">*완전 함수적 종속 : 기본키가 아닌 컬럼중에 특정 기본키에만 종속하는 <b>부분적 종속</b>이 없는 상태. </p>

     1차 정규화가 된 table에서 기본키는 (student, subject). 그 이유는 학생과 과목을 알아야 레코드를 구분할 수 있기 때문이다.

     그렇다면 기본키가 아닌  age속성은 student와 subject 두 개의 속성 모두에 종속인지 확인해보면 된다. age속성는 **student에만 종속**되어있기 때문에 **2차 정규화의 조건**인 **완전함수적 종속**에 위배된다.  <span style="font-size:.9em; color: grey">즉, age 속성은 student 의 이름만 알아도 찾을 수 있는 속성</span> 

     ​	

     <center><span style="color:grey; font-size:.8em">2차 정규화 전</span></center>

     | student                              | age                               | subject                                      |
     | ------------------------------------ | --------------------------------- | -------------------------------------------- |
     | 보리                                 | 1                                 | 수학<span style='color: white'>, 과학</span> |
     | <span style='color: red'>보리</span> | <span style='color: red'>1</span> | <span style='color: red'>과학</span>         |
     | 쌀                                   | 2                                 | 수학                                         |
     | 수수                                 | 3                                 | 수학                                         |

     ​	

     <center><span style="color:grey; font-size:.8em">2차 정규화 <span style="color:red">후</span></span></center>

     _*학생 테이블_

     | student | age  |
     | ------- | ---- |
     | 보리    | 1    |
     | 쌀      | 2    |
     | 수수    | 3    |

     _*과목 테이블_

     | student | subject                              |
     | ------- | ------------------------------------ |
     | 보리    | 수학                                 |
     | 보리    | <span style='color: red'>과학</span> |
     | 쌀      | 수학                                 |
     | 수수    | 수학                                 |

     ​			

     ​		

   - **3차 정규화** : 기본키를 제외한 속성들 간의 **이행적 함수 종속**이 없는 경우

     <p style="font-size:0.9em; text-align:right; font-style:italic;">*이행적 함수 종속 : 기본키를 제외한 다른 속성이 특정 속성을 결정지으면 안된다는 것</p>

     ​		

     <center><span style="color:grey; font-size:.8em">3차 정규화 전</span></center>
     
     | 학생id | 학생이름 | 생년월일 | <span style='color: red'>주소</span> | 시   | 동   | 구   |
   | ------ | -------- | -------- | ------------------------------------ | ---- | ---- | ---- |
     |        |          |          |                                      |      |      |      |

     와 같이 컬럼이 있을 때 "<span style='color: red'>주소</span>"만 알면 "시", "동", "구"도 알 수 있다. 

     <center><span style="color:grey; font-size:.8em">3차 정규화 <span style="color:red">후</span></span></center>

     _*학생 테이블_
     
     | 학생id | 학생이름 | 생년월일 | <span style='color: red'>주소</span> |
   | ------ | -------- | -------- | ------------------------------------ |
     |        |          |          |                                      |

     _*주소 테이블_
     
     | <span style='color: red'>주소</span> | 시   | 동   | 구   |
   | ------------------------------------ | ---- | ---- | ---- |
     |                                      |      |      |      |
     
     위 테이블 처럼 주소속성으로 구분지어 정규화 한다.
     
     ​	
     
   - <span style='color: red'>정규화를 한다고 해서 무조건 <b>(조회)</b>성능이 하락하는 것도 아니다.</span> 성능에 대한 이슈는 어떻게 테이블을 구성할 것이며, 어떤 데이블 들이 있는지 등 고려해야하는 사항이 많다. 즉 **함수의 종속성(Functional Dependency)에 근거한 정규화 수행이 필요하다.**

     > **함수의 종속성(Functional Dependency)** <span style="font-size:.9em; color: grey">- ex)  2차 정규화</span>
     >
     > 데이터들이 어떤 기준값에 의해 종속되는 현상을 지칭. 이때 기준값을 결정자(Determinant)라 하고 종속되는 값을 종속자(Dependent)라고 한다. 

     













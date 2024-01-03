# What is train_test_split()?


​	

# What is 'train_test_split()'?

> AI 모델링을 위해 X_train, X_test, y_train, y_test로 나누는 작업은 필수적입니다. 
> 저는 train과 test를 나누는 코드를 직접 짜서 사용했었는데 scikit learn에서 제공되는 함수가 있었습니다. <b>"train_test_split()"</b>이를 알아보겠습니다. 

​	

​	

직접 사용되는 모습을 보며 이해해보겠습니다.

```python
import numpy as np
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(data, label, test_size=0.3, random_state=29)
```

​	

​	

### train_test_split() 함수안에 사용되는 인자에 대하여 하나씩 알아보겠습니다. 

1. data는 AI에게 학습시킬 문제 리스트입니다.

2. label은 AI에게 학습시킬 문제의 정답 리스트입니다.

3. test_size는 float 또는 int 값을 받습니다. 

   1. float의 경우 : 0.0 ~ 1.0 사이의 값을 받습니다. 이는 train과 test 세트를 나누기 위한 test 데이터의 비율을 의미합니다.

   2. int의 경우 : 모집단의 수에서 몇 개의 데이터를 test 테이터로 설정할 것인지를 의미합니다.

   3. 입력하지 않을 수도 있는데 이때는 임의의 비율로 train과 test를 나누어 줍니다.

      ​	

4. <b>random_state</b>는 해당 함수를 수행할 때마다 동일한 결과를 얻기 위한 나만의 비밀번호 같은 개념입니다. 

   - train_test_split(..., <span style='color:red'>test_size=0.3</span>) 과 같은 함수는 train은 70% , <span style='color:red'>test는 30%</span>의 데이터 세트를 추출합니다. 하지만 추출된 데이터는 수행을 할때마다 다를 수 있습니다. <b>random하게 70%, 30%를 추출하기 때문입니다.</b>

   - 가령 1~ 100까지 일련번호로 된 100개의 데이터를 train_test_split(.., <span style='color:red'>test_size=0.3</span>) 로 수행하면 해당 함수를 첫번째 수행할 때는 train은 1~70, <span style='color:red'>test는 71~100</span>이 될 수 있지만, 다시 수행하면 이번에는 train은 31~100 , <span style='color:red'>test는 1~30</span>이 될 수 있습니다. <br>

   - 70%, 30% 로 나누는건 동일하지만 함수를 수행 시마다 추출한 레코드들을 달라질수 있습니다. 내부적으로 70%, 30%로 나눌때 random 함수를 적용하기 때문입니다.

   - random_state=1 이라고 하면 바로 이 random함수의 seed 값을 고정시키기 때문에 <b>여러 번 수행하더라도 같은 레코드를 추출</b>합니다. random함수의 seed값을 random_state라고 생각하시면 됩니다.

   - random_state는 어떤 값으로 설정해도 상관없습니다. 이는 random값을 고정하는 역할만 수행하기 때문입니다.

     ​	

     ​	

     ​	

     ​	

# 👀요약

함수는 기존에도 알고 있었지만, 함수가 동작하는 방식을 정확히 알지 못하면 사용하기가 꺼려졌었어서 이참에 조금 알아보았습니다. 그리고 알보던 중 random_state가 어떤 의민지 몰라서 같이 정리해보았습니다. 실은 random_state를 알아보는 것이 이번 포스팅의 핵심이었습니다. 

​	

​	









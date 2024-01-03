# What is Data_Scaling


​	

# 데이터 스케일링(Data Scaling) 이란?

>**데이터**의 값의 범위를 조정하는 것

​		

​		

​	

## 🤔 왜 데이터 스케일링을 하는가?

> 데이터의 경향, feature의 영향, 상관관계 등 데이터를 분석하는데 활용할 수 있으며, 인공지능 개발 중 학습단계에서 여러 feature의 영향도를 비슷한 수준으로 맞추기 위해 범위를 조정.

​		

아래 그림을 보면서 이해해보겠습니다.

<image src="/images/Data_Scaling.assets/image-20220406172937054.png" width="100%" style="display: block; margin: 20px auto">

### 과제

- 여러 feature(성별, 나이, 몸무게, 자산현황)의 특성을 보고 <span style="color: red">Label</span>을 예측하는 모델을 만든다고 가정

### 문제점

- 값의 범위를 보면 
  - 성별 0, 1  
  - 나이 0~100, 
  - 몸무게 0~120, 
  - 자산현황 0~700,000,000,
  - <span style="color: red">Label은 0~800,000,000의 값을 갖는다고 가정</span>한다면,  

​		

<b>성별, 나이, 몸무게</b>는 <span style="color: red">Label</span>를 예측하는데 큰 영향이 없는 형태로 학습될 수 있습니다.

아마도 **자산현황**이 <span style="color: red">Label</span>을 예측하는데 가장 큰 영향을 미친다고 판단하며 학습될 것 같습니다. 

(feature들 간, 값의 규모의 차이가 너무 크기 때문)

<span style="color:grey; font-size:0.8em">또, 데이터의 값이 <b>너무 크거나 혹은 작은 경우</b>에 모델 학습과정에서 <b>0으로 수렴하거나 무한으로 발산</b>할 수 있습니다.</span>

​		

### 결론

> feature별 데이터의 규모(scale)이 다르다면, 논리적인 인공지능 학습이 이루어지지 않을 수 있습니다.
>
> <b>따라서, 데이터 스케일링 작업을 통해, 모든 feature의 범위(또는 분포)를 유사하게 만들어 주는 것이 모델 학습에 좋은 영향을 주게 됩니다.</b>

​	

​	

​			

​	

​	

## 🧐 스케일링 방법은 어떤 것들이 있는지?

> 데이터 스케일링은 `데이터 값의 규모`를 `비슷한 수준으로 맞추어 주는 것`입니다. 그리고 `비슷한 수준으로 맞추어 주는 방법`은 주로 통계적인 기법을 사용해 진행합니다.

<span style="color: grey; font-size:.8em; float:right">* scikit-learn에서 제공되는 라이브러리 기준으로 정리</span>

​	

​	

 `scikit-learn`에서는 `MinMaxScaler`, `MaxAbsScaler`, `StandardScaler`, `RobustScaler` <br>총 4가지의 스케일링 방법을 제공합니다.

- `MinMaxScaler` : 데이터 값을 0 ~ 1 사이로 스케일링 <b>(정규화 *Normalization*)</b>
- `MaxAbsScaler` : 데이터 값을 -1 ~ 1 사이로 스케일링
- `StandardScaler` : 데이터의 평균 = 0, 분산 = 1이 되도록 스케일링 <b>(표준화 *Standardization*)</b>
- `RobustScaler` : 데이터의 중앙값 = 0, IQE = 1이 되도록 스케일링

​		

​		

### MinMaxScaler (Normalization)

> 최대 최소값을 기준으로 스케일링

- <image src="/images/Data_Scaling.assets/image-20220411093738903.png" width="300px" style="display: block; margin: 20px auto">

- 데이터를 0 ~ 1 사이의 값으로 스케일링. (모든 스케일링은 열 기준으로 적용 <span style="color: grey; font-size:0.8em">( 성별, 나이, 몸무게, 자산현황 별로 계산 )</span> 됩니다.)
- 데이터의 최소값과 최대값을 알 때 사용합니다. 왜냐하면
- 이상치가 존재할 경우 스케일링 결과가 매우 좁은 범위로 압축될 수 있기 때문입니다.
  - 최대값과 최소값의 차이를 기준으로 설정하기 때문에, `최소값이 매우 작거나, 최대값이 매우 큰 이상치가 있을 때 특이점이 발생할` 수 있습니다.

- 분류보다 `회귀에 유용`합니다.

```python
from sklearn.preprocessing import MinMaxScaler

# minmax scaler 선언 및 학습
minmaxScaler = MinMaxScaler().fit(X_train)

# train셋 내 feature들에 대하여 minmax scaling 수행
X_train_minmax = minmaxScaler.transform(X_train) 

# test셋 내 feature들에 대하여 minmax scaling 수행
X_test_minmax = minmaxScaler.transform(X_test)

# 스케일링 된 결과 값으로 본래 값을 구할 수도 있다.
# X_origin = minmaxScaler.inverse_transform(X_train_minmax)
```

<image src="/images/Data_Scaling.assets/image-20220411141427818.png" width="250px" style="display: block; margin: 20px auto"><image src="/images/Data_Scaling.assets/image-20220411141537572.png" width="250px" style="display: block; margin: 20px auto;">

​	

​	

​	

​	

### MaxAbsScaler

> value 절대값 기준, 최대 절대값을 100%로 두고 스케일링 (음수 표현 가능)

- 데이터가 -1 ~ 1 사이에 위치하도록 스케일링.
- 절대값의 최소값 = 0, 절대값의 최대값 = 1이 되도록 스케일링 합니다.
- 데이터의 값이 양수만 존재할 경우 `MinMaxScaler`와 유사하게 동작합니다.
- 이상치에 매우 민감합니다. (value의 범위가 큰 쪽에 존재할 경우 매우 민감)

```python
from sklearn.preprocessing import MaxAbsScaler

# MaxAbsScaler 선언 및 학습
maxabsScaler = MaxAbsScaler().fit(X_train)

# train셋 내 feature들에 대하여 maxabs scaling 수행
X_train_maxabs = maxabsScaler.transform(X_train)

# test셋 내 feature들에 대하여 maxabs scaling 수행
X_test_maxabs = maxabsScaler.transform(X_test)

# 스케일링 된 결과 값으로 본래 값을 구할 수도 있다.
# X_origin = maxabsScaler.inverse_transform(X_train_maxabs)
```

<image src="/images/Data_Scaling.assets/image-20220411141427818.png" width="250px" style="display: block; margin: 20px auto"><image src="/images/Data_Scaling.assets/image-20220411141508494.png" width="650px" style="display: block; margin: 20px auto;">

​		

​		

​		

​		

### StandardScale (Standardization)

> 정규분포를 따르게 스케일링

- <image src="/images/Data_Scaling.assets/image-20220411093753724.png" width="200px" style="display: block; margin: 20px auto">
- 데이터의 평균 = 0, 분산 = 1이 되도록, 즉 데이터가 표준 정규 분포(standard normal distribution)를 따르도록 스케일링.
  - (x - x의 평균) / (x의 표준편차)
- 데이터의 최소값과 최대값을 모를 때 사용합니다.
- 평균(mean)과 분산(variance)을 사용합니다.
- 모든 feature들이 같은 스케일을 갖게 됩니다.
- 평균과 표준편차가 이상치로부터 영향을 많이 받는다는 점에서 이상치에 민감합니다.
- 회귀보다 `분류`에 유용합니다.

```python
from sklearn.preprocessing import StandardScaler

# standard scaler 선언 및 학습
standardScaler = StandardScaler().fit(X_train)

# train셋 내 feature들에 대하여 standard scaling 수행
X_train_standard = standardScaler.transform(X_train)

# test셋 내 feature들에 대하여 standard scaling 수행
X_test_standard = standardScaler.transform(X_test)

# 스케일링 된 결과 값으로 본래 값을 구할 수도 있다.
# X_origin = standardScaler.inverse_transform(X_train_standard)
```

<image src="/images/Data_Scaling.assets/image-20220411141427818.png" width="250px" style="display: block; margin: 20px auto"><image src="/images/Data_Scaling.assets/image-20220411141642215.png" width="650px" style="display: block; margin: 20px auto;">

​	

​	

​	

​	

### RobustScale

- 데이터의 중앙값 = 0, IQE(사분위값) = 1이 되도록 스케일링 합니다.
  -  IQE(사분위값, 1/4, 3/4에 위치한 값) =  IQR = Q3 - Q1 : 즉, 25%과 75%의 값

- 중앙값(median)과 IQR(interquartile range)을 사용합니다.
  - `RobustScaler`를 사용할 경우 `StandardScaler`에 비해 스케일링 결과가 더 넓은 범위로 분포합니다.
  - 양수만 가진 데이터의 경우 `StandardScaler`와 동일한 결과를 도출합니다.
- 모든 feature들이 같은 스케일을 갖게 됩니다.
- 이상치의 영향을 최소화(거의 영향을 받지 않음) 합니다.
  - 사분위 값을 사용하기 때문에 양극값은 제외됨.

- 회귀분석에 조금 더 유용합니다.

```python
from sklearn.preprocessing import RobustScaler

# RobustScaler 선언 및 학습
robustScaler = RobustScaler().fit(X_train)

# train셋 내 feature들에 대하여 robust scaling 수행
X_train_robust = robustScaler.transform(X_train)

# test셋 내 feature들에 대하여 robust scaling 수행
X_test_robust = robustScaler.transform(X_test)

# 스케일링 된 결과 값으로 본래 값을 구할 수도 있다.
# X_origin = robustScaler.inverse_transform(X_train_robust)
```

<image src="/images/Data_Scaling.assets/image-20220411141427818.png" width="250px" style="display: block; margin: 20px auto"><image src="/images/Data_Scaling.assets/image-20220411143252151.png" width="600px" style="display: block; margin: 20px auto;">

​	

​	

​	

​	



# AI학습 때 왜 .fit() 한 뒤에 train, test data를 .transform()할까?


​	

## 🤔 왜 .fit() 한 뒤에 train, test data를 .transform()할까?

> 결론은 <b>.fit()을 통해 학습데이터(train)의 경향을 파악 및 저장하고 이 경향을 train, test data를 적용해 조정해주기 위함입니다.</b>

* minmaxScaler를 대표로 .fit()한 결과를 출력해보면 

<image src="/images/Data_Scaling.assets/image-20220411143534625.png" width="500px" style="display: block; margin: 20px auto">



.fit()이 어떤 역할을 하는지 알 수 없습니다. 그냥 MinMaxScaler()라는 객체가 할당된 것만 알 수 있습니다. 

[공식문서](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html#sklearn.preprocessing.MinMaxScaler)를 살펴보면

> fit(*X*, *y=None*)
>
> Compute the minimum and maximum to be used for later scaling.

<span style="color: grey; font-size:.8em; float:right">* StandardScale()의 경우 Compute the mean and std to be used for later scaling. 평균과 분산을 저장

​	

나중에 스케일링할 때 사용할 최대 최소의 값을 계산한다고 합니다. 

어떤 동작을 하는지는 알았는데 **왜 이런 과정이 필요할까? 왜 train data의 경향을 뽑아 test에 적용할까?**

​	

​	

해당 내용은 스케일링 보다 <b>인공지능 학습에 대한 설명</b>일 수도 있습니다. 

우리가 인공지능을 통해 알고 싶은 것은 <b>어떤 경향(분포)를 학습한 모델이 특정 input값이 입력되었을 때, 신뢰성 있는 예측값</b>입니다.

이 과정을 세분화해 알아보자면, 

1. train_data의 경향(분포)를 학습한 모델
2. 특정 input(test_data)값이 입력
3. 모델이 예측한 결과 도출

​		

AI 모델은 1번 과정을 통해 학습합니다. 예를 들어 train_data가 1~100으로 이루어져 있고 이를 학습했다면, 

2번의 특정 input값이 입력되었을 때, <span style="color: red;">**특정 input의 위치는 train_data의 1~100의 경향에 빗대어 보니 어디쯤 이다.**</span>는 결과를 도출합니다.



<image src="/images/Data_Scaling.assets/image-20220418134619188.png" width="500px" style="display: block; margin: 20px auto">

즉, 만약  train_data와 test_data에서 각각 .fit()하여 .transform() 하게 된다면, 

test_data의 규모만 줄여주며, 이를 train_data에 빗대어 예측할 수 없게 됩니다.

​	

그래서 test_data를 train_data의 분포(경향)을 따르게 .transform() 해준 후 모델 예측(.predict())에 넣어 결과를 보는 것입니다. 

​	

​	

​		

​	

​	

​	

​	

​	

#### 추가 사항 > fit_transform()

위의 프로세스대로 스케일링을 진행한다면 

1. train의 경향 추출(.fit()) 
2. 2.train의 경향을 train에 반영하는 스케일링 진행(.transform()), 
3. train의 경향을 test에 반영하는 스케일링 진행(.transform())

총 3번의 과정을 거처야 하나, 		

이를 줄이기 위해 fit_transform()이라는 함수를 통해 1, 2 과정을 한 번에 진행할 수도 있습니다. 

​	

​	

​	

​	


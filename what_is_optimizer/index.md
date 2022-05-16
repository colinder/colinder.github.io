# What is optimizer


​	

![img](C:\Users\kjong\Desktop\JG\03_Blog\content\posts\AI\what_is_optimizer.assets\img.png)

​	

## 옵티마이저의 목적

> 옵티마이저는 학습 데이터(Train data)셋을 이용하여 모델을 학습 할 때 <b>데이터의 실제 결과</b>와 <b>모델이 예측한 결과</b>를 기반으로 <b>오차를 잘 줄일 수 있게</b> 만들어주는 역할입니다.

​	

따라서 <b>최적화(Optimization)</b>은 손실 함수(Loss Function)의 결과값을 최소화하는 모델의 파라미터(가중치)를 찾는 것을 의미합니다. 

​	

​	

​	

## 옵티마이저 리스트

1. 경사 하강법(Gradient Descent)
2. 확률적 경사 하강법(Stochastic Gradient Descent, SGD)
3. Momentum
4. Nesterov Accelerated Gradient (NAG)
5. Adam
6. AdaGrad
7. RMSProp
8. AdaMax
9. Nadam

<span style="float:right; font-size:0.8em; color:grey">참고 https://keras.io/api/optimizers/</span>

​	

​	

## Loss?

**loss**는 손실함수를 의미합니다. 

입력 데이터로 부터 도출한 결과(예측값)가 Label값(정답)과 얼마나 일치하는지 평가해주는 함수를 의미합니다. 

여기서는 손실함수를 'mse'를 사용하겠다는 의미가 됩니다. mse는 평균제곱오차(mean squared error)를 의미합니다. 나중에 자세히 다룰 내용이고 여기서는 그냥 얼마나 예측과 다른지 평가해주는 값이라고 생각하시면 됩니다. 예측값과의 차이를 의미하므로 작으면 작을수록 좋은 모델이라는 의미가 됩니다.

 	

​	

**optimizer**는 손실 함수를 기반으로 네트워크가 어떻게 업데이트될지 결정합니다. 여기서는 adam을 사용하였습니다.

옵티마이저 종류 더 알아보기 -> https://keras.io/ko/optimizers/

​	

​	

​	

​	

​	



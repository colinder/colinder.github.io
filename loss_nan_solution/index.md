# 모델 학습 시 loss가 Nan으로 나올 때 해결법


​	

## 모델 학습 시 loss 값이 Nan으로 나올 때 해결 방법

- if df == pandas.DataFrame()
  	df.isnull().any()로 데이터셋에 NaN이나 inf 값이 들어있는지 확인한다.

- 다른 optimizer들을 사용해본다. *(ex. sgd, adam, nadam)*

- 다른 activation function을 사용해본다.

  - 사용하는 모델마다 적합한 activation function이 있습니다. 
  - 즉. 모델에 대하여 공부해야하고, 적합한 activation function을 찾아 학습해야 합니다.

- learning rate(학습률)을 낮춰본다.

  - learnin rate는 batch_size와 연관이 있습니다.

  - 러닝레이트 줄이기 vs 배치사이즈 키우기

    - 결과론적으로 배치 사이즈를 키우는 건 러닝 레이트를 줄이는 거랑 동일한 효과를 나타낸다.

      출처: https://honeyjamtech.tistory.com/43 [취미생활하는 공대생]

  - [Learning rate & batch size best 조합 찾기 (feat.논문리뷰와 실험결과) 참고](https://inhovation97.tistory.com/32)

- 데이터 스케일링(정규화 등)을 한 후 시도 한다. 

​	

​	

​	

​	



​	



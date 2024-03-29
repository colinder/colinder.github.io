# Overfitting


​	

# 과적합(Overfitting)

> [잘 정리된 내용](https://velog.io/@yookyungkho/%EB%94%A5%EB%9F%AC%EB%8B%9D%EC%9D%98-%EA%B3%A0%EC%A7%88%EB%B3%91-Overfitting%EA%B3%BC%EC%A0%81%ED%95%A9-%ED%95%B4%EA%B2%B0-%ED%8C%81)을 발견하였고 거기에 개인적인 경험을 추가하기 위해 정리하였습니다. (대부분 필사입니다.)
>
> 인공지능 모델링을 하다보면 항상 만나게 되는 과적합을 어떻게 해결해야하는지, 방법에 대하여 정리

인공지능에게 흔히 말하는 <b>깊이있는 공부</b>를 시키려면 모델의 layer을 늘리고, 노드(unit)을 늘리는 방법을 떠올린다. 그리고 그러다 보면 train data에 만 너무 적합하게, 처음 보는 test data를 집어넣었을 땐 형편없는 결과를 도출하는 모델이 된다. (이를 과적합이라 한다.)

일반적으로 과적합은 모델 학습과정에서 **valid loss가 지속적으로 감소하다가 증가하는 지점부터 발생**한다고 정의된다. 하지만 처음부터 valid loss가 증가하는 모델도 많이 보았다. 

​			

​	

​	

# Solution

과적합을 해결하는 가장 기본적인 방법은 train data의 양을 늘리는 것이다.

다만 실무를 경험해보면 알겠디만, 데이터를 늘리고 싶다고 늘릴 수 있는게 아니다. 다양한 방법으로 기존의 데이트를 늘리는 방법을 적용해보기도 하지만, 과연 재구성한 데이터의 적합성과 유효성을 알긴 어렵다.

​	

### 1. Dropout (드롭아웃)

> 중간 노드의 학습을 임의로 중지하는 것. 학습하는 layer의 노드 중 일부를 랜덤으로 꺼버려 역전파시 파라미터 업데이트를 막는다. 이는 모형의 <b>불확실성을 증가</b>시켜 과적합 해결에 기여한다.

드롭아웃 비율을 얼마로 해야 하는가에 정답은 없다. 개인적으로 **과적합이 심하지 않다면 0.1~0.5를,  심하다 싶으면 0.7~0.8로 설정**한다.

다만, 드롭아웃의 의미를 알고 <b>얼마나 꺼볼까?</b> 하는 개념을 가지고 진행하는 것을 추천한다.

또 여러 layer 마다 드롭아웃을 다르게 줄텐데 <b>불확실성을 높인다.</b>는 논리하에서 

노드의 수가 많은 층의 드롭아웃을 우선 적용하고 비율도 높게 주고 있다. 다만 결과는 보장하지 못한다. 😒

​	

​	

### 2. L1(Lasso)/L2(Ridge) Regularization

*학습에 기여하지 못하는 모수를 0으로 만들어버리자*로 접근하는 방법

아래 수식과 같이 손실함수에 람다항을 추가해서 일종의 페널티를 주는 방법.

​	

- L1 Regularization(절댓값)

  <image src="/images/overfitting.assets\image.png" width="500px" style="display: block; margin: 20px auto">

- L2 Regularization(제곱)

  <image src="/images/overfitting.assets\image-16722134873871.png" width="500px" style="display: block; margin: 20px auto">

자세한 증명은 [이 블로그](https://dailyheumsi.tistory.com/57)를 참고

내가 중요하게 학습 시키고 싶은 정보는 **regularization을 가급적 출력층에 사용하는 것이 좋다**는 것이다. 교수님께서는 regularization이 **통계학의 가설검정과 유사**하다고 하셨는데, 가설검정 시 t값이나 F값, 혹은 p-value를 통해 유의미한 변수인지 아닌지를 판단하는 것처럼 regularization은 **loss를 줄이는데 기여하지 못하는 모수를 0(L1) 또는 0에 가까운(L2) 값으로 제한**하는 기능을 한다. 보통 L2 regularization이 L1보다 더 많이 쓰인다. 

​		

> 여기서 잠깐!
> **<Q. Dropout과 Regularization을 동시에 사용해야 하는가?>**
>
> 이 질문에 대한 답변은 교수님 버전과 구글링 버전이 달라서 혼란스러웠다.
>
> - 교수님: 두 기법은 상호보완관계, 하나만 쓰면 안되고 둘 다 써야한다.
>
> - 구글링: 두 기법은 상호보완 관계, 보통 둘 다 쓰는게 성능이 더 높은데 하나만 쓸지 둘 다 쓸지는 경험적으로 판단할 문제다. 즉, 케바케! 데바데!
>
>   실제 실험을 해보았을 때에는 구글링 답변처럼 모델마다 결과가 달랐다. 어떤 모델은 두 기법 모두 쓰는게 효과가 좋았고 또 어떤 모델은 하나씩만 쓰는게 더 좋았다. 개인적으로 구글링 입장에 동의하고, 일반적으로 둘 다 쓰는게 성능이 더 높다고는 하니 둘 다 실험해보는 것을 추천한다.

​	

​	

### 3. 출력층 직전 은닉층의 노드 수를 줄여라.

*출력직전 모수를 줄이는게 효과가 좋았다.*

통계학에서 오버피팅을 해결하는 방법은 **쓸데없는 변수를 제거해서 입력변수 x의 수를 줄이는** 것인데(t-test 등을 통해 significant하지 않은 변수를 제거한다) 통계학의 관점에서 **출력층 직전 은닉층 노드 수는 설명변수의 수**가 된다. 따라서 의미있는 설명변수들을 남기기 위해/생성하기 위해 출력직전 노드 수를 확 줄여버리는 것이다.

나는 주로 출력 범주의 수가 20이하인 경우, FCL(Fully Connected Layer) 마지막 층의 노드 수를 32로 줄인다. 만약 출력 범주의 수가 100 이상으로 넘어갈 경우엔 그에 맞춰서 마지막 층 노드 수를 결정한다.

cf) 비슷한 이야기로 **과적합을 막으려면 모수(parameter)의 수를 줄여야** 하는데, **은닉층 자체를 줄이거나 은닉층 노드 수를 줄이면** 된다. 이때 터무니없이 막 줄이면 안되고, 조절해보면서 적당한 선을 찾아야 한다. CNN에서는 kernel size, pooling size, filter 수를 줄이면 된다.

​	

​	

### 4. Batch Normalization(배치정규화)

뉴럴넷에서 **각 활성함수의 미분값은 역전파 과정에서 계속 곱해지기 때문에 굉장히 중요**하다. 시그모이드 함수의 경우 일정수준 이상 혹은 이하의 값이 입력되었을때 미분값이 0에 가깝게 되는데, 이때 파라미터 업데이트과정에서 0에 가까운 값이 지속적으로 곱해지면 **vanishing gradient(기울기 소실)** 문제가 발생한다. 이렇게 되면 **파라미터 업데이트가 거의 일어나지 않고 수렴 속도도 아주 느리게 되어 최적화에 실패**하게 되는데, 이 문제를 해결하는 방법으로는 배치정규화 외에도 **relu** 등의 활성함수를 사용하거나 **가중치 초기화(weight initialization)**을 적용하는 방법이 있다.

배치정규화는 **mini batch 별로 분산과 표준편차를 구해 분포를 조정**한다. 역전파시 파라미터 크기에 영향을 받지 않기 때문에 좋다고 한다.

배치정규화는 각 은닉층에서 활성함수 적용 직전에 사용되어야한다. 일반적으로 **선형결합-배치정규화-활성함수-드롭아웃** 순으로 은닉층 연산이 진행된다.

​	

​	

### 5. 그 외 참고하면 좋을 사항들

**1. epoch의 증가는 과적합 해결을 위한 수단이 아니라 모니터링 수단이다.**

단순하게 epoch만 늘려주면 train data의 loss는 줄고 accuracy는 높아지기 때문에 train data의 loss와 accuracy는 valid 결과와 비교하기 위한 참고사항일 뿐이다. epoch 수를 늘리면 valid와 train loss가 교차하는 지점이 발생한다. 일반적으로 valid loss가 감소하다가 증가하는 시점을 과적합으로 정의하기 때문에 이 지점에서 적당한 epoch을 결정한다. 이는 early stopping과도 관련이 있다.

​	

**2. epoch이 증가하면서 train loss 와 valid loss가 수렴해야 가장 좋다.**

교차점에서 epoch을 결정하더라도 train과 최종 test loss와 accuracy를 비교해야한다. 이러한 점검은 cross-validation으로 진행해야한다.

​	

**3. batch_size는 과적합과 관련이 없다. 모수의 수렴 문제와 관련이 있다.**

배치사이즈가 작을 수록 수렴속도는 느리지만 local minimum에 빠질 가능성은 줄어든다. 반면 배치사이즈가 클 수록 학습진행속도와 수렴속도가 빨라지지만 항상 빨리 수렴하는 것은 아니다. 작은 데이터셋이라면 32가 적당하다고 하는데 이 역시도 구글링하다보면 수많은 의견들이 존재한다. 적당히 참고해가면서 실험해보면 좋겠다.

​	

​	

​	

## 참고 

- https://velog.io/@yookyungkho/%EB%94%A5%EB%9F%AC%EB%8B%9D%EC%9D%98-%EA%B3%A0%EC%A7%88%EB%B3%91-Overfitting%EA%B3%BC%EC%A0%81%ED%95%A9-%ED%95%B4%EA%B2%B0-%ED%8C%81

​	


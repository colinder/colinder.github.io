# 편향(Bias) & 분산(Variance)


​	

# 편향(Bias)과 분산(Variance)

> 인공지능 모델링을 하면 정답을 맞추기 위해 컴퓨터는 여러 번의 예측값을 내놓는데, **컴퓨터가 내놓은 예측값의 동태를 묘사하는 표현**이 '편향'과 '분산'입니다.

​	

​		

​	

# 🤔결론부터 말하자면

예측값들과 정답이 대체로 멀리 떨어져 있으면 **편향이 놓다**고 말하고,<br>예측값들이 서로 멀리 흩어져 있으면 **분산이 높다**고 말합니다.

​	

​	

​	

<center>아래 그림을 보면</center>

<image src="/images/Bias&Variance.assets/rq_iMVSuIK1K4ykF9RQnF05hH6xxWm3lmNPWuQ3hfK9r4-3GBIuCxCW3L7QH53M3EIwbVWOcaRiRLDc0AIJ-0uq8-qzavpSWPceQ1lchq-ZPF16l3KLst24-x6MbGYFqQbEJmEI3gEc.png" width="600px" style="display: block; margin: 20px auto">

1. 왼쪽 상단 과녁은<br>
   예측값들이 대체로 정답 근방에서 왔다갔다 합니다. > 편향이 낮습니다.<br>
   예측값들끼리 서로 몰려 있습니다. > 분산이 낮습니다.
2. 오른쪽 상단 과녁은 <br>
   예측값들이 대체로 정답 근방에서 왔다갔다 합니다. > 편향이 낮습니다.<br>
   예측값들끼리 서로 흩어져 있습니다. > 분산이 높습니다.
3. 왼쪽 하단 과녁은<br>
   예측값들이 대체로 정답으로부터 멀어져 있습니다. > 편향이 높습니다.<br>
   예측값들끼리 서로 몰려 있습니다. > 분산이 낮습니다.
4. 오른쪽 하단 과녁은<br>
   예측값들끼리 대체로 정답으로부터 멀어져 있습니다. > 편향이 높습니다.<br>
   예측값들끼리 서로 흩어져 있습니다. > 분산이 높습니다.

​			

​			

​		

## 수식으로 표현하면 아래와 같습니다.

> 수식으로 굳이 알아보는 이유는 정확하게 알기 위함입니다. 너무 핵심만을 전달해 버리면 구체적인 내용을 모를 수도 있고. 이는 '안다.'고 말하기에는 부족하기 때문이죠.

​	

<image src="/images/Bias&Variance.assets/9376.png" width="600px" style="display: block; margin: 20px auto">

- X 는 입력된 데이터를 의미합니다.
- <span style="color:red">f(x) (붉은 점)</span>는 <span style="color:red">우리가 맞추고자하는 정답</span>을 의미합니다.
- <span style="color:blue">f(x)에 ^ 표시가 있는 것(파란 점)</span>은 f hat x 라고 읽습니다.
  hat은 머리에 쓰는 모자 라는 뜻인데, 수학에서는 '특정 값' 을 지칭할때 사용합니다.<br>
  여기서는 <span style="color:blue">컴퓨터가 내놓은 값(예측값)</span>을 의미합니다.

- <span style="color:grey">E[_]</span><span style="color:grey">(회색 점)</span>는 기대값(expectation)을 의미(평균)합니다. <br>
  <span style="color:blue">파란점으로 표시된 f^(x)는 예측값'들'</span> 이므로, <span style="color:grey">여러 예측값'들'의 평균</span>을 의미합니다.

​		

​	

## '편향'을 수식으로 바꿔보면

<image src="/images/Bias&Variance.assets/9379.png" width="600px" style="display: block; margin: 20px auto">

<center style="color:grey; font-size:0.8em">붉은점과 회색점의 관계를 나타냅니다.</center>

​	

​	

<image src="/images/Bias&Variance.assets/9378.png" width="600px" style="display: block; margin: 20px auto">

<center style="color:grey; font-size:0.8em">보통 위와 같이 계산해서 파악합니다.</center>

​	

**편향**이란, <b>예측값과 정답이 얼마나 차이가 있는가?</b>를 표현합니다.

- E[f^*(x)] : <span style="color:grey">예측값<b>들</b></span>의 평균 입니다. <span style="color:grey">(위 그림에서 회색 점)</span>
- f(x): <span style="color:red">정답값</span> 입니다. <span style="color:red">(위 그림에서 빨간 점)</span>

둘을 빼면 정답과 예측값이 서로 떨어진 거리를 알 수 있습니다. 그리고 

**이 거리는, 정답과 예측값들이 서로 얼마나 떨어져 있는지 알려주는 지표가 됩니다.**

​	

그런데 f(x)에 제곱이 붙어 있습니다. 

왜냐하면, 어떤 예측값은 정답보다 <b>클 것</b>이고, 어떤 예측값은 정답보다 <b>작을 것</b>이기 때문에 
<b>두 값 사이의 거리는 양수가 나오기도, 음수가 나오기도 하기 때문</b>입니다.

제곱을 해서 모두 양수를 만들어 주면, 값들을 '쌓는 것'이 가능해집니다. 그럼 얼마나 쌓였는지를 나중에 따로 잴 수가 있죠.

꼭 제곱을 사용해야만 하는 것은 아니고, <b>절대값을 씌워 주는 것도 좋은 방법</b>입니다.<br>
양수를 만들기 위한 목적이니까, 4제곱, 6제곱을 해 주어도 관계없습니다. 다만 그러면 계산이 복잡해지고 무의미하게 커져, <b>가장 쉬운 방법으로 거리를 재는 수식에서는 제곱을 해 주는 것이 그냥 상식처럼 되었습니다.</b>

​	

​	

​	

# '분산'을 수식으로 바꿔보면

<image src="/images/Bias&Variance.assets/9377.png" width="600px" style="display: block; margin: 20px auto">

<center style="color:grey; font-size:0.8em">파란점과 회색점의 관계를 나타냅니다.</center>

​	

​	

<image src="/images/Bias&Variance.assets/12458.jpg" width="600px" style="display: block; margin: 20px auto">

<center style="color:grey; font-size:0.8em">수식은 '파랑 점 하나(예측값)' 를 x로 보고 표현한 식입니다.</center>

​	

<span style="color:blue">파란점</span>과 <span style="color:grey">회색점(파란점의 평균)</span>의 차를 보는데 혹시 음수일 수 있으니,<br> 제곱하고 평균을 내어 분산을 확인합니다.

​	

​	

​	

​	

## '편향'과 '분산'은, 머신러닝 모델이 '복잡하게 생긴 정도'와 큰 관련이 있습니다.

​		

### 회귀 모델의 경우

<image src="/images/Bias&Variance.assets/K3fjd0dVZbfXCo5LHFZVfSdfq5UBM-iLniMQZ3V4TqByZON9LvmtYazglVlcN9kyVXc5ZpZT2BJhd_yLWzZ8kcYku8yyEn2PaHdEn-JBobtPcWWG26WVUXdClvTf_o9nv835wghYCoY.png" width="600px" style="display: block; margin: 20px auto">

세 그래프는, 세 가지의 서로 다른 머신러닝 모델로 같은 데이터를 설명하는 모습입니다.<br>

- 정답들은 <b>'굵은 점'</b>으로 찍혀 있습니다. 
- 모델이 내놓은 예측값은 <b>'연속된 작은 점'</b>으로 표현되어 직선 혹은 구불구불한 곡선 보입니다.

**이 문제에서는 여러 점들의 경향을 <u>잘 표현하는 모델</u>을 찾는 것이 목적입니다.**

​	

**첫 번째 그래프**를 보면, 

- 데이터들<span style="font-size:0.8em">(**'굵은 점'**)</span>이 모델의 예측값과<span style="font-size:0.8em">(**'연속된 작은 점'**)</span> 멀어져 있으므로 편향(bias)이 높고, 
  모델의 예측값<span style="font-size:0.8em">(**'연속된 작은 점'**)</span>들 끼리는 별로 떨어져 있지 않게 되므로(왜냐면 같은 직선위의 점들이니까) 분산(variance)은 낮습니다.

**세 번째 그래프**를 보면, 

- 정답들이 모델과 아주 붙어 있으므로 편향이 낮고,
- 모델의 예측값들 끼리는 매우 흩어져 있게 되므로(왜냐면 구불구불한 선 위의 점들이니까) 분산이 높습니다.

**두 번째 그래프** 정도가 적당하다.고 볼 수 있습니다.

​	

​	

---

### 분류 모델의 경우	

<image src="/images/Bias&Variance.assets/k7fm8oOB8fY3S57dOzc1MUsRjeSLSBKX8ZvMOvV3JZKXRzrM7OhCvIF2En5ahfzJHcW8uxDaF0bd3dYc4_HK52JVXd_SJ6pVrAOjHm1b3QSSC9Ne_t5VmrKto15BmF_kn4mN_64dirU.png" width="600px" style="display: block; margin: 20px auto">

- 정답들은 <span style="color:red">빨강 동그라미</span> 혹은 <span style="color:green">초록 십자가</span>로 찍혀 있고,
- 모델이 이 둘을 분류(구분)하는 기준(예측선)은 직선 혹은 구불구불한 곡선으로 표현되어 있습니다. 

**이 문제에서는 <span style="color:red">빨강 동그라미</span>와 <span style="color:green">초록 십자가</span>를 <u>잘 구분하는 모델</u>을 찾는 것이 목적입니다.**

​	

**첫 번째 그래프**를 보면, 

- 데이터들이 모델과 멀어져 있으므로 편향(bias)이 높고, 
- 모델이 구분한 기준선은 별로 떨어져 있지 않아(왜냐면 같은 직선위의 점들이니까) 분산(variance)은 낮습니다.

**세 번째 그래프**를 보면, 

- 정답들이 모델과 아주 붙어 있으므로 편향이 낮고,
- 모델이 구분한 기준선은 매우 흩어져 있어(왜냐면 구불구불한 선 위의 점들이니까) 분산이 높습니다.

**두 번째 그래프** 정도가 적당하다 고 볼 수 있습니다.

​	

​		

​		

---

### Underfitting과 Overfitting

회귀 문제이든, 분류 문제이든

- **첫 번째 그래프**와 같은 상황을 **Underfitting**
- **세 번째 그래프**와 같은 상황을 **Overfitting**이라고 합니다.

모델이 너무 단순하게 생겼으면(=훈련이 너무 덜 되어 있으면) => 정답을 잘 내놓지를 못하고

모델이 너무 복잡하게 생겼으면(=훈련이 너무 심하게 되어 있으면), => 훈련용 데이터에만 너무 최적화 되어 있어, 새로운 문제(데이터)는 틀린 답을 내놓을 가능성이 높습니다.

​	

훈련이 알아서 <span style="font-size:1.2em">**적당히 잘**</span>되면 좋겠지만, 이 부분은 개발자들의 영역입니다.

​	

​		

​	

### Model Complexity

<image src="/images/Bias&Variance.assets/lAbzDl1HYiYHAEuGnaUw2GdCyQzkZvjWisgNY-ZRYqvRG-X-U7f7cL_UunIF7v5q0BbUSw4CZ-1-xMXs8mvE8fbGa7ghFeEGzuwJ6wiIs64nUgJxkDNEC2JrSTUHEjViRZLdA23NLqI.png" width="600px" style="display: block; margin: 20px auto">

편향(Bias)과 분산(Variance)은<br>
한쪽이 증가하면 다른 한쪽이 감소하고,<br>
한쪽이 감소하면 다른 한쪽이 증가하는 경향을 보입니다. <span style="font-size:1.2em"><b>"반비례적 관계"</b></span>



모델이 데이터를 반복 학습하는 횟수가 늘어날수록 > 모델이 복잡한 정도(Model Complexity)도 따라서 늘어나게 되는데, <br><b>이것은 훈련용 데이터를 그대로 외우는 방향이기 때문.</b>

따라서 <b>Training Error(loss)</b>는 갈수록 줄어들게 되지만, <b>Validation Error(val_loss)</b>는 어느 정도까지는 줄어들다가, 어느 지점 이후부터는 다시 상승하게 됩니다.

**하여 모델을 훈련시키는 도중에 Validation Error가 최소인 지점에서 훈련을 멈추는 것이 필요**합니다.

​	

​	

​	

​	

​	

---

### 해당 내용은 아래의 블로그 내용을 필사하며 정리한 내용입니다.

https://opentutorials.org/module/3653/22071

​	

​	

​	

​	



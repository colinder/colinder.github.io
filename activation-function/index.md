# Activation_Function


​		

# Activation Function(활성화 함수)

>입력 신호의 총합을 출력신호로 변환하는 함수를 일반적으로 Activation Function이라고 합니다.

​		

아직 잘 모르겠습니다. 하여 인공신경망에 빗대어 이해해보겠습니다. 

<image src="https://t1.daumcdn.net/cfile/tistory/99EAB2505AE487821C" style="display: block; margin: 10px auto;">

인공 신경망은 인간의 중추신경계(뇌)속의 뉴런들이 정보를 전달하고 학습하여 결과를 도출해내는 과정을 모방한 학습알고리즘입니다.

여기서 의문점이 생깁니다.

- 예를 들어 지나가다 귤을 보았는데 맛이 있을지 없을지 어떻게 알 수 있을까요? 저 귤의 데이터를 수집해 봅시다.

- `타원형`, `이쁜 노란색`, `푸르른 꼭지` 라는 데이터를 눈으로 보고 뇌속의 뉴런들이 이 정보를 분석해 `맛있겠다.`라는 출력을 불러왔습니다.

입력된 데이터를 어떤 <b>규칙? 방법</b>에 따라 변환하여 출력 데이터로 변환하였는데요. 이 역할을 해주는 것이 ***Activation Function***입니다.


​		

​		

​	

## ✋ ‘Node와 Layer’가 어떻게 생겼는지 다시 알아봅시다.

<image src="/images/Node_Layer_00.png" width="1000px" style="display: block; margin: 20px auto">

<p style='text-align:right; font-style:italic; color:grey'><u>Node</u>는 <u>뇌속의 뉴런</u>과 같은 역할이고,<br><u>Node</u>들이 모여 <u>인공신경망의 층이라 불리는 Layer</u>가 됩니다.</p>

​			

​		

​		

​		

# Features of activation function

> 활성화 함수는 주로 <b>비선형 함수(곡선 그래프로 표현되는 함수)</b>를 사용합니다.

<img src="https://kejdev.github.io/assets/images/data-science.jpg" style="display: block; margin: 10px auto;">

왜 그럴까요?

_<b>선형함수(직선 그래프로 표현되는 함수)를 사용하면 Layer(층)을 깊게 쌓아 학습하는 의미가 줄어들기 때문입니다.</b>_

예를 들어 어떤 현상(or 데이터)를 분석(or 학습)하는데 선형(linear)함수 <b>H(x) = `a`x</b> 활성화 함수로 사용하는 <b>3-Layer</b> 네트워크가 있을 때.

- 해당 네트워크는 결국 <b>Y(x) = H(H(H(x)))</b>와 동일해집니다. 간단히 Y(x) = <b>a^3</b>x 로도 표현할 수 있습니다.

- 하나의 Layer<span style="font-size:0.8em; color:grey">( Y(x) = <b>a^3</b>x )</span>로도  <b>3-Layer</b>를 대신할 수 있게 됩니다.

  _즉, 선형(linear)적인 연산을 갖는 Layer는 몇개를 쌓는다 해도, 결국 하나의 <b>선형적인 연산처리하는 Layer로 대체될 수 있</b>습니다._

- 또 위의 그림을 보면 <span style="color: blue">파란원(특이점)</span>을 더 많이 만족하는 것은 <b>비선형 함수</b>가 됩니다. 

- 실제 자연환경에서는 선형적이지 않은 복잡한 일(특이점)들이 더 많기 때문에 다양한 조건에 대응하기 위해서 비선형 함수를 사용하는 것입니다.  

  ​	
  
  ​	

​			

​			

# Type of activation function

> 대표적인 활성화 함수에 대하여 알아보겠습니다.

활성화 함수는 엄청 많습니다.  **Threshold**, **Binary step**, **Sigmoid**, **Tanh or Hyperbolic tangent**, **Softmax**,  **ReLU**, **Leaky ReLU**, **Parametric ReLu**,  **ETC.....**

활성화 함수는 '<b>하이퍼 파라미터</b>'라고 불립니다. 간단히 <b>사용자가 설정해주는 값</b>인데, 결국 정답은 없으며, 우리의 직관? 판단?으로 설정해야하는 값입니다. 

막연하지만, 함수의 특징을 안다면 상황에 맞게 최적의 함수를 선택해 사용할 수 있으니 알아보겠습니다.

​	

​	

### <center>🔗 Binary Step function</center>

<image src="/images/Activation_Function_00.png" width="600px" style="display: block; margin: 20px auto">

- 마치 계단 같이 0을 기준으로 0 or 1로 극명하게 값을 결정됩니다. => <b>Output은 0 또는 1</b>

- 0.01도 1로 처리되어 100배의 결과값 차이가 발생합니다.

- 마치 선형(linear)함수와 비슷해 학습할 내용이 많으면 사용하지 않는 것이 좋습니다.

  ​				

  ​	

  ​	

  ​	

---

#### 🤔 <span style="color:grey">너무 극단적인 구분값말고 융통성있는 </span>이진 분류<span style="color:grey">가 필요하다면?</span>

​	

### <center>🔗 Sigmoid function</center>

<image src="/images/Activation_Function_01.png" width="600px" style="display: block; margin: 20px auto">

- 선형함수의 결과를 0 ~ 1까지의 비선형 형태로 변환하는 함수입니다. => <b>Output은 0~1 사이의 실수</b>

- 로지스틱, 정규분포 등과 같이 <b>이진 분류 문제(1 or 0 /성공 or 실패...)</b>의 확률 표현에 자주 사용됩니다.

- binary classification의 출력층 노드에서 0~1사이의 값을 만들고 싶을때 사용합니다.

  ---

- ✨인공 신경망이 학습하는 과정에서 _저번엔 안 중요했는데, 이번에는 중요하네?_ 와 같이 학습하는 요소의 <b>가중치</b>를 잘 반영해야 하는데 Sigmoid는 **원점이 중심이 아닙니다.(Not zero-centered)**. <b>평균이 0.5이며 항상 양수를 출력</b>하기 때문에 출력의 가중치합이 입력의 가중치합보다 커질 가능성이 높아집니다. 이것을 <b>편향 이동(bias shift)</b>이라고 하는데, 이러한 이유로 **각 Layer를 지날 때마다 분산이 계속 커져** 가장 높은 Layer에서는 활성화 함수의 출력이 그래프의 극값인 0 or 1로 수렴하게 되고 결국 기울기가 0이 되어 버리는 **Gradient Vanishing**이 일어나게 됩니다.

- 또 애초에 매우 높거나, 낮은 입력에 대한 예측을 거의 변경하지 않아<span style="font-size:0.8em; color:grey">(크면 무조건1, 작으면 무조건 0)</span> 궁극적으로 신경망이 학습을 거부하게 됩니다.


​				

​		

​	

​	

---

#### 🤔 <span style="color:grey">Sigmoid보다 가중치를 조금 더 잘 반영하는 </span>이진 분류<span style="color:grey">가 필요하다면?</span>

​	

### <center>🔗 Tanh(Hyperbolic tangent) function</center>

<image src="/images/Activation_Function_02.png" width="600px" style="display: block; margin: 20px auto">

- Hyperbolic Tangent(tanh) 함수는 Sigmoid의 대체제로 사용되는 함수입니다. 매우 유사하게 생겼습니다.

- 실제로, Hyperbolic Tangent 함수는 확장 된 Sigmoid 함수입니다.

- 차이점은 출력범위가 <span style="color: red">0 ≤</span> Sigmoid ≤ 1이고 <b><span style="color: blue">-1 ≤</span> tanh ≤ 1</b> 이라는 점입니다. 

- <b>원점 중심(zero-centered)</b>이기 때문에, 시그모이드와 달리 <b>편향 이동</b>이 일어나지 않습니다. 하지만, tanh함수 또한 입력의 절대값이 클 경우 -1이나 1로 수렴하게 되므로 **Gradient Vanishing**가 발생하지만, Sigmoid보다 발생 경향이 적어 효과적으로 사용이 가능합니다.

  ---

- 보통 Sigmoid와 비교하는데 주로 미분한 결과를 보며 설명합니다. <b>왜 미분한 결과 일까요?</b><br>
  <b>_∴ <u>표현할 수 있는 기울기</u>가 결국 <u>출력될 수 있는 값</u>이기 때문입니다._</b>

<image src="/images/Activation_Function_03.png" width="600px" style="display: block; margin: 20px auto">

<p style='text-align:right; font-style:italic; color:grey'><u>tanh그래프의 미분계수</u>의 최댓값은 1입니다.<br><u>sigmoid</u>와 비교하여 <u>최대값이 약 4배가 크고</u>이는 약 4배 더 다양한 분석이 가능하다고 해석할 수 있습니다.</p>

​	

​	

​	

​	

---

#### 🤔 <span style="color:grey">만약 이진분류가 아닌 </span>다중 분류<span style="color:grey">가 필요하다면?</span>

​	

### <center>🔗 Softmax function</center>

<image src="/images/Activation_Function_04.png" width="300px" style="display: block; margin: 40px auto">

- 세 개 이상으로 분류하는 다중 클래스 분류에서 사용되는 활성화 함수입니다.

- N개의 class로 분류한다고 가정했을 때 Softmax함수는 들어오는 값들을 확률값으로 바꿔줍니다. 

  예를 들어 어떤 이미지가 주어지고 이것이 사과인지, 배인지, 귤인지 판단해야 할 때, Softmax함수는 <b>사과일 확률, 배일 확률, 귤일 확률</b>을 출력해줍니다. 즉, <b>Output은 class 별 확률</b>이고 <b>class별 출력값의 총합은 1</b>이 되어야합니다.

- class별로 확률을 알려주다보니, <b>출력 노드</b>에서 확률을 확인하고 이를 최종결과로 반영하는 상황에서 <b>많이 사용</b>됩니다.

  ---

- 근데, 위의 Softmax공식을 보니 **자연 상수e**를 사용하고 있습니다. 왜 '자연 상수e'일까요? 그 이유는 크게 2가지가 있습니다.

  1. 미분이 쉽습니다. <span style="font-size:0.8em; color:grey">( == 값을 계산하기 용이하다.)</span>

     <image src="/images/Activation_Function_07.png" width="200px" style="display: block; margin: 10px auto 18px  auto">

  2. 큰 값은 더 크게, 작은 값은 더 작게 가중치를 주어 확률을 계산합니다.

     예로 <u><b>0.1, 1.0, 2.0</b></u>의 입력값을 Softmax로 출력해보면  <u>0.03, 0.33, 0.63</u>가 아닌 <b>0.1, 0.2, 0.7</b>이 나옵니다.<br> Softmax는 1과 2가 아닌 <b>e</b>^1 <span style="font-size:0.8em; color:grey">(2.718)</span>,  <b>e</b>^2 <span style="font-size:0.8em; color:grey">(7.389)</span>로 확률을 계산합니다. 즉 입력값이 커짐에 따라 기울기가 증가하며 더 큰 차이를 만들며, <b><i>Soft하게 "Max"한 값을 선정하는데 용이</i></b>합니다.

     

  

​	

​		

​		

---

#### 🤔 <span style="color:grey"> </span>Gradient Vanishing 문제가 없는<span style="color:grey"> 활성화 함수가 필요하다면?</span>

​	

### <center>🔗 ReLU(Rectified Linear Unit) function</center>

<image src="/images/Activation_Function_05.png" width="600px" style="display: block; margin: 40px auto">

- <b>가장 많이 사용되는 활성화 함수</b> 중 하나입니다.

- 음수를 입력하면 0을 출력하고, 양수를 입력하면 입력값을 그대로 반환 => <b>Output은 0 이상의 실수</b>

- tanh함수 대비 약 6배 빠른 속도로 학습이 가능하고, **Gradient Vanishing**가 발생하지 않습니다.

- 주로 hidden Layer에 사용됩니다.

  ---

- ReLu의 문제점은 <u>음수 값은 무조건 0으로 처리하기 때문에 학습 능력이 감소</u>한다.는 것에 있습니다.<br>
  이를 죽은 렐루(Dying ReLU)라고 합니다.

- 그리고 이 문제를 완화하기 위해 <b>Leaky ReLu</b>함수가 등장하였습니다.

  ​			

  ​	

  ​	

  ​	

---

#### 🤔 ReLU의 <span style="color:grey">음수값을 살린</span>를 학습법이<span style="color:grey"> 필요하다면?</span>

​	

### <center>🔗 Leaky ReLU function</center>

<image src="/images/Activation_Function_06.png" width="600px" style="display: block; margin: 40px auto">

- 위 그래프에서 공식 <b>αx (x ≤ 0)</b>에서 α는 사용자가 지정하는데 '<b>매우 작은 수</b>'로 지정하며 보통 <b>0.01</b>로 설정합니다.
- ReLU에서 음수값을 물이 누수(Leaky)되듯이 매우 작게나마 표시해주는 함수입니다.

​	

​	

​	

​	

​	

​	

​	

​	

이밖에도 많은 활성화 함수가 있으니 더 찾아보는 것을 추천합니다. 

<image src="https://miro.medium.com/max/1192/1*4ZEDRpFuCIpUjNgjDdT2Lg.png" style="display: block; margin: 40px auto">

​	

​	

​	

# 👀참고

- 일반적으로는 ELU -> LeakyReLU -> ReLU -> tanh -> sigmoid 순으로 사용한다고 한다. cs231n 강의에서는 ReLU를 먼저 쓰고, 그 다음으로 LeakyReLU나 ELU 같은 ReLU Family를 사용하며, sigmoid는 사용하지 말라고 하고 있습니다.

- chain rule은 두 함수를 합성한 합성 함수의 도함수(derivative)에 관한 공식입니다.<br>
  <b>chain rule = 연쇄법칙 = 합성함수의 미분법.</b>
  
  > 합성 함수 
  >
  > 두 개 이상의 함수를 하나의 함수로 결합하여 만들어진 함수. 어떤 함수 속에 또 다른 함수가 들어있고, 그 또 다른 함수 속에 다른 함수가 들어있다. 
  
  
  
  
  
  



​	

​	



## 참고한 글 

- https://kejdev.github.io
- https://wooono.tistory.com/209
- https://inhovation97.tistory.com/30


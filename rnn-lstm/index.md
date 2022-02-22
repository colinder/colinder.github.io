# RNN & LSTM


​	

# <span style="color:#01DF3A">RNN</span> & <span style="color:#E3A437">LSTM</span>

>감정분석을 위한 프로젝트를 기획하면서 RNN과 LSTM이라는 신경망 분석에 대하여 알게 되었습니다. 신경망 분석은 기계학습과 인지과학에서 영감을 얻어 설계된 통계학적 학습 알고리즘인데요. 이 둘의 개념를 알아보겠습니다. 

​		

​			

## <span style="color:#01DF3A">RNN</span>(Recurrent Neural Network) 순환 신경망

> 시퀀스(Sequence)라는 것이 있습니다. 
>
> <b>시퀀스는 간단히 '문맥이 있는 데이터' 입니다.</b> 문장은 단어들의 시퀀스, 음악은 음계들의 시퀀스, 동영상은 이미지의 시퀀스라고 할 수 있습니다. 그런데 시퀀스의 길이는 가변적입니다. 기존의 뉴럴 네트워크 알고리즘은 이미지처럼 고정된 크기의 입력을 다루는 데는 탁월하지만, 가변적인 크기의 데이터를 모델링하기에는 적합하지 않았습니다.
>
> 그리고 이런 시퀀스 테이터를 처리하기 위해 고안된 모델을 <b>시퀀스 모델</b>이라고 하고, 그 중에서도 <span style="color:#01DF3A">RNN</span>은 Deep Learning에 있어 가장 기본적인 모델입니다. <span style="color:#01DF3A">RNN</span>이 기존의 뉴럴 네트워크와 다른 점은 ‘기억’(다른 말로 hidden state[은닉층])을 갖고 있다는 점입니다.
>
> 새로운 입력이 들어올때마다 네트워크는 자신의 기억을 조금씩 수정합니다. 결국 입력을 모두 처리하고 난 후 네트워크에게 남겨진 기억(hidden state)은 시퀀스 전체를 요약하는 정보가 됩니다. 이 과정은 새로운 단어마다 계속해서 반복되기 때문에 RNN에는 Recurrent, 즉 순환적이라는 이름이 붙었으며, 이런 반복을 통해 긴 시퀀스를 처리할 수 있는 것입니다.

아직 이해가 어렵습니다. 동작하는 모습을 보며, 더 알아보겠습니다.

<image src="https://t1.daumcdn.net/cfile/tistory/99E349505BD1987801" style="display: block; margin: 10px auto">



<span style="color:#01DF3A">RNN</span>은 입력(input)을 받아 출력(Output)를 만들고, 이 <b><span style="color:#EE863A">출력을 다시 입력으로 받</span></b>습니다. 일반적으로 RNN을 그림으로 나타낼 때는 위의 그림처럼 하나로 나타내지 않고, 아래의 그림처럼 각 **타임 스텝**(time step) `t`마다 **순환 뉴런**을 펼쳐서 타임스텝 별 입력과 출력을 나타냅니다.

<image src="https://t1.daumcdn.net/cfile/tistory/997C79465BD1989C14" style="display: block; margin: 30px auto" width="1000px">

​			

## <span style="color:#01DF3A">RNN</span> 학습 방법

<span style="color:#01DF3A">RNN</span>은 순환 구조이므로 **hidden state<span style="font-size:0.8em">(layer)</span>의 데이터를 저장**하고 있습니다. 일반적인 인공신경망과 비슷하게 **Gradient Descent**와 **backpropagation**을 이용해 학습을 하는데, 시간의 흐름에 따른 작업이기 때문에 backpropagation을 확장한 <b>BPTT(Back-Propagation Through Time, 역전파)</b>을 사용해서 학습을 합니다.

<p style="text-align:right; font-size:0.9em; color:grey; font-style:italic">*Gradient Descent(경사 하강법) : 최적해를 구하는 알고리즘. <br>기본 개념은 함수의 기울기를 구하고 경사의 절대값이 낮은 쪽으로 계속 이동시켜 극값에 이를 때 까지 반복시키는 것.</p>

즉, <b><span style="color:#01DF3A">RNN</span>은 스스로를 반복하면서 과거 자신의 정보(가중치)를 기억하고 이를 학습에 반영합니다.</b>

​	

#### 🤔다만, 

<span style="color:#01DF3A">RNN</span>이 시간을 거슬러 올라가면서 학습을 하는데 **과거로 올라가면 올라갈수록 gradient값이 계산이 잘 되지 않습니다.** 그리고 Vanishing Gradients Problem이 발생합니다. gradient는 곱연산으로 이루어져 있는데, **미분값 즉 변화량이 매우 작다면, 데이터를 효과적으로 학습시키지 못하**고, error rate가 미쳐 다 낮아지지 못한채 수렴해버리는 문제가 발생합니다.

_간단히, **길이가 긴 Sequence data 의미를 잘 파악하지 못하며, 짧은 Sequence 데이터만 의미있는 학습이 진행됩니다. == 기억력이 좋지 못한 모델**_

​	

그리고 이를 해결하기 위해 LSTM이 등장했습니다.	

## <span style="color:#E3A437">LSTM</span>(Long Short-Term Memory) 장,단기 메모리

잠깐 <span style="color:#01DF3A">RNN</span>의 동작원리를 다시 살펴보면, 아래와 같습니다.

<image src="/images/RNN_04.png">



위 그림은 싱글 레이어(단일 tanh 레이어)를 가지고 반복되는 <span style="color:#01DF3A">simple RNN</span> 모듈을 표현하고 있습니다. 

그럼 <span style="color:#E3A437">LSTM</span>네트워크는 어떻게 생겼을까요?

<image src="/images/LSTM_00.png" width="800px" style="display: block; margin: 10px auto" >

Cell의 내부는 

<image src="/images/LSTM_01.png" width="600px" style="display: block; margin: 10px auto" >

RNN에 비해 복잡한 구조를 가지고 있습니다. 차근히 알아보겠습니다.

​		

위의 그림에서 <image src="/images/LSTM_cell_state.png" height="25px"> 이 **(cell) state** / <image src="/images/LSTM_hidden_state.png" height="25px">이 **hidden state**입니다. 

- <i><b>Cell state</b></i>

  (cell) state는 컨베이어 벨트 처럼 담겨있는 정보(state)를 계속 흐르게 하는 부분입니다. (cell) state에는 뭔가를 더하거나 없앨 수 있는데 이는 gate라는 곳에서 제어됩니다. 

- <i><b>Hidden state</b></i>

  hidden state는 '**이전 출력물(previous output)**'입니다. RNN에서 확인했던 <b><span style="color:#EE863A">출력을 다시 입력으로 받</span></b>는 부분입니다.

  ​	

<span style="color:#E3A437">LSTM</span>의 내부에는 '3가지 gate'와 '1번의 업데이트'가 진행됩니다.

- 망각게이트(forget gate) : 장기기억에서 필요없는 정보를 **삭제**하는 게이트

- 입력게이트(input gate) : 장기기억에 저장할 정보를 **입력**하는 게이트 

- Update :  forget gate에서 삭제할 정보를, input gate에서 저장할 정보를 결정했고 이를 **더해**주는 단계			

- 출력게이트(output gate) : **다음 시점으로 전달할 state(단기상태)를 결정**하는 게이트

  ​		

단계별로 확인해보겠습니다.

### 1. 망각게이트(Forget gate)

<image src="/images/LSTM_forget_gate.png" width="1000px" style="display: block; margin: 20px auto">



[이전 output, 현재 input]를 종합해 cell state으로 전달되는 '어떤 값'을 계산합니다. / 다만 이 계산의 활성함수가 σ([시그모이드](https://ko.wikipedia.org/wiki/%EC%8B%9C%EA%B7%B8%EB%AA%A8%EC%9D%B4%EB%93%9C_%ED%95%A8%EC%88%98))로 진행되기 때문에 전달되는 값은 '0 ~ 1'이 됩니다.

​	\> '0'일 경우, 이전의 cell state값은 모두 '0'이 되어 미래의 결과에 아무런 영향을 주지 않음<br>
​	\> '1'일 경우, 미래의 예측 결과에 영향을 주도록 이전의 cell state 값(Ct-1)을 그대로 보내 완전히 유지함

> 즉, Forget Gate는 [현재 입력과 이전 출력]을 고려해서, cell state의 어떤 값을 버릴지/지워버릴지('0'이 출력되면 날려버림) 결정하는 역할을 합니다.

​			

​			

### 2. 입력게이트(Input gate)

<image src="/images/LSTM_input_gate.png" width="1000px" style="display: block; margin: 20px auto">



input gate는 현재 정보를 기억하기 위한 게이트입니다. forget gate와 동일하게 [이전 output, 현재 input]를 종합해 <image src="/images/LSTM_input_00.png" height="25px"> 라는 필터를 만들고 <image src="/images/LSTM_input_01.png" height="25px"> 을 cell state에 얼마나 등록할지 결정하는 부분입니다.

​		

​			

### 3. Update

<image src="/images/LSTM_Update.png" width="1000px" style="display: block; margin: 20px auto">

Update는 지난 두 과정[forget gate, input gate]을 통해 cell state에 반영하는 과정입니다.

​		

​			

### 4. 출력게이트(Output gate == hidden state)

<image src="/images/LSTM_output_gate.png" width="1000px" style="display: block; margin: 20px auto">

최종적으로 cell state에 반영된 값을 얼마나 출력할지? 결정하는 구간입니다.

​	

​	

​	

​	

​	

# 👀요약

> 딥러닝에 있어 가장 기본적인 모델이라는 <span style="color:#01DF3A">RNN</span>과 <span style="color:#E3A437">LSTM</span>에 대하여 정리하였습니다. <br>기술을 사용하는 것은 이론을 학습하는 것보단 난이도가 낮다는 평가가 지배적입니다. 즉 그냥 가져다 쓸 수는 있지만, 개념적으로 어떤 분석이 되고 어떤 학습이 되는지 이해하고 사용하는 것과 아닌 것과는 분명 차이가 있을 것입니다. 간단하게라도 어떤 프로세스로 진행되는 지 알아보면 좋을 것 같습니다.

​	

​	

​	

## 참고 했던 글

- https://www.jksmer.or.kr/articles/xml/x2eO/

- https://dreamgonfly.github.io/blog/understanding-rnn/

- https://brunch.co.kr/@gdhan/2

- https://ratsgo.github.io/natural%20language%20processing/2017/03/09/rnnlstm/

- https://m.blog.naver.com/PostView.nhn?blogId=magnking&logNo=221311273459&proxyReferer=https:%2F%2Fwww.google.com%2F

- https://www.edwith.org/deeplearningchoi/lecture/15840?isDesc=false

- http://colah.github.io/posts/2015-08-Understanding-LSTMs/

- https://excelsior-cjh.tistory.com/183

  ​	

  ​	

  ​	

  ​	

  


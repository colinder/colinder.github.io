# ROC and AUC


​	

# ROC 곡선이란?

> Receiver operating characteristic 수신기동작특성? 번역하기에 어려운 용어같습니다. 다만 인공지능의 성능을 파악하는데 자주 사용되기 때문에 어떤 개념이고 어떤 의미인지, 어떻게 해석해야 하는지 정리합니다.
>
> ROC(Receiver Operating Characteristic) = 모든 임계값에서 분류 모델의 성능을 보여주는 그래프

​		

​	

## 🤔 결론부터 정리하면

1. 주로 <b style="font-size: 1.2em">분류</b>로 결과를 확인하는 인공지능 성능 평가 방법으로 자주 쓰임. 
2. 그래프로 보자면 <b style="color:blue; font-size:1.2em">좌상단(파란 점)에 가까울수록</b> 좋은 성능을 보이는 모델. 

<image src="/images/ROC_curve.assets\img-16475750679541.png" width="500px" style="display: block; margin: 20px auto">

​	

또 그래프의 x축, y축을 보면 "__FALSE POSITIVE RATE__", "__TRUE POSITIVE RATE__"라고 적혀 있는데요. 이들은 <span style="font-size:1.3em; color:purple">__Confusion matrix__</span>라는 분류에 주로 사용되는 검증 방식에 등장하는 내용입니다. 

아래의 표를 보면서 계속하겠습니다.





예를 들어 <b>강아지 인가 아닌가?</b> 를 맞추는 모델을 만든다고 가정.

데이터는 <b>강아지</b>와 <b>강아지가 아닌 것</b>이 섞여 있음.

모델 예측값으론 <b>강아지다. 강아지가 아니다.</b>가 있을 것입니다.

​	

여기서 

<b>강아지인 데이터를(True Class - Positive)</b> <span style="color:#a8d08d">강아지라고 예측(Predicted Class - Positive)한 것</span>이 있을 것이고,  <span style="color: #fb8975">강아지가 아니라고 예측(Predicted Class - Negative)한 것</span>이 있습니다. 

또 <b>강아지가 아닌 데이터를(True Class - Negative)</b> <span style="color:#a8d08d">강아지라고 예측(Predicted Class - Positive)한 것</span>이 있을 것이고, <span style="color: #fb8975">강아지가 아니라고(Predicted Class - Negative)한 것</span>가 있을 겁니다.



전 이를 <b>맞맞, 맞틀, 틀맞, 틀틀</b> 이라고 말합니다. <span style="color: grey; font-size:0.8em">(저만 이렇게 말합니다.)</span>

<span style="font-size:1.3em; color:purple">__Confusion matrix__</span>에 대한 내용은 다음 글을 참고 하시고,





위의 표를 다시 보면

<image src="/images/ROC_curve.assets\1_fxiTNIgOyvAombPJx5KGeA1.png" width="500px" style="display: block; margin: 20px auto">



<span style="color: blue"><b>파란색 네모</b></span>의 부분을 그래프화 한 값이 <b>ROC</b>입니다. 

즉 ROC_cerve는 

AUROC값은 0.5 ~ 1 사이의 값을 가집니다. 0.5 이하의 값을 가지도록 하는 임계치는 폐기해야 합니다. 반면 1에 가까울수록 효율적인 판단 임계치이며. 0.7 미만의 경우 차선으로 고려할 수 있는 정도이며, 0.7~0.8은 좋은 정도 0.8 이상은 훌륭한 정도로 봅니다.

​	

​	

​	

​	


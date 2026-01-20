<script src="/livereload.js?mindelay=10&amp;v=2&amp;port=1313&amp;path=livereload" data-no-instant defer></script># How to evaluate AI?


​	

<span style="font-size:1.3em; color:purple">__Confusion matrix__</span>는 <b>분류</b>에 주로 사용되는 검증 방식에 등장하는 내용입니다. 

아래의 표를 보면서 계속하겠습니다.

![1_fxiTNIgOyvAombPJx5KGeA-16475750679553](C:\Users\kjong\Desktop\JG\03_Blog\content\posts\AI\how_to_evaluate_AI.assets\1_fxiTNIgOyvAombPJx5KGeA-16475750679553.png)

예를 들어 <b>0, 1의 정답을 맞추는 모델</b>을 만든다고 가정.

데이터는 <b>0</b>와 <b>1</b>이 섞여 있음.

모델은 예측값으로 <b>0, 1</b>을 뱉을 것입니다.

​	

여기서 

<b>정답이 1인 데이터를(True Class - Positive)</b> <span style="color:#a8d08d"><b>1로 예측(Predicted Class - Positive)한 것</b></span>이 있을 것이고,  <span style="color: #fb8975"><b>0으로 예측(Predicted Class - Negative)한 것</b></span>이 있습니다. 

또 <b>정답이 0인 데이터를(True Class - Negative)</b> <span style="color:#a8d08d"><b>1로 예측(Predicted Class - Positive)한 것</b></span>이 있을 것이고, <span style="color: #fb8975"><b>0으로 예측(Predicted Class - Negative)한 것</b></span>이 있을 겁니다.



전 이를 <b>맞맞, 맞틀, 틀맞, 틀틀</b> 이라고 말합니다. <span style="color: grey; font-size:0.8em">(저만 이렇게 말합니다.)</span>

​	

그리고 이 경우의 수를 활용해 <span style="font-size:1.2em"><b>모델의 성능평가에 가장 기본적으로 사용되는 개념들이 등장</b></span>합니다. 

일반적으로 생각할 수 있는 모델의 성능에 대하여 고민하자면.



### 이 모델은 정답을 얼마나 잘 맞추는가?

1. Accuracy (정확도)
   - <b>정확도</b>란 전체 데이터 중 모델이 <b>0을 0이라고 예측한 것</b>과, <b>1을 1이라고 예측한 것</b>의 비율입니다.
   - ![img](C:\Users\kjong\Desktop\JG\03_Blog\content\posts\AI\what_is_confusion_matrix.assets\99745F3F5BE0613D1A-16475750679556.png)

​		

> 단순히 모델이 얼마나 정확한 예측을 하는가를 보기에는 좋지만, 확인해야할 사항이 있습니다. 

🤔 만약에 모델에게 줄 문제 100개가 있다고 가정했을 때, 정답 구성이 <b>1이 10개, 0이 90개</b> 가있는데 학습이 비정상적으로 되어 예측값을 0으로만 도출하는 <b>모델의 경우 정확도는 무려 90%가 됩니다. </b>

​			

이런 경우, <b>모델이 정답 1을 얼마나 맞추었는가? 비율을 알면 해결할 수 있습니다.</b>

​	

2. Recall (재현율)

   - <b>재현</b>이라는 표현은 모델이 얼마나 정답을 잘 재현하였는가?

   - 정답 == 참값 데이터(강아지 데이터), 재현 == 맞춘 비율

   - **재현율**이란 강아지 데이터 중에서 모델이 강아지라고 예측한 것의 비율입니다. 

   - 즉, **재현율**이란 참값 데이터 중에서 모델이 참값을 맞춘 비율입니다.

   - ![997188435BE05B0628-16475750679555](C:\Users\kjong\Desktop\JG\03_Blog\content\posts\AI\how_to_evaluate_AI.assets\997188435BE05B0628-16475750679555.png)

1. Precision (정밀도)
   - <b>정밀</b>이라는 표현은 모델의 정답이 얼마나 참값안에 있는가?
   - 모델의 정답 == 하나의 군집 / 참값안 == 정답의 군집
   - **정밀도**란 모델이 정답이라고 분류한 것 중 진짜 정답이 맞는 것은 얼마나 되는가?
   - 모델이 강아지라고 분류한 것 중에서 진짜 강아지 데이터인 것의 비율입니다. 
   - ![99F66B345BE0596109-16475750679554](C:\Users\kjong\Desktop\JG\03_Blog\content\posts\AI\how_to_evaluate_AI.assets\99F66B345BE0596109-16475750679554.png)



재현율과 정밀도는 약한 반비례 관계를 이룹니다.



3. Accuracy (정확도)

- 

1. F1-score


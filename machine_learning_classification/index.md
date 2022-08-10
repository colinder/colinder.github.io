# Machine_learning type


​	

#  Machine learning (ML)

> 머신러닝을 활용하기에 좋은 분야에 대하여 알아보겠습니다.

​			

1. 분류(classification)

   > 정해진 카테고리 중 어디에 속하는지 알아내는 경우

   - 종류

     1. KNN(k-nearest neighbor)

        > k-nearest neighbor는 데이터를 분류하고 새로운 데이터 포인트의 카테고리를 결정할 때 K 개의 가장 가까운 포인트를 선점하고 그중 가장 많이 선택된 포인트의 카테고리로 이 새로운 데이터를 분류하는 방법이다. 

        중요 개념

        - 좌표와 같이 데이터의 위치를 특정할 수 있게 변환

        - 내 주변에 가까운 K개의 **데이터와의 거리**를 재고 그 중 더 많은 값에 내가 속한다고 판단.

        - K는 홀수로 등록(짝수의 경우 class 가 50:50으로 나뉘면 내가 어디 속하는지 판단할 수 없음.)

        - .fit() 없이 데이터를 바로 가져와서 분석 가능 == Lazy model

        - **데이터와의 거리**를 재는 방식은 여러개가 있으니 상황에 맞게 선택하여 사용

        - 예를 들어 **실수 데이터의 경우 유클리드 거리 측정 방식**을 사용하고, 범주형 혹은 **이진 데이터와 같은 유형의 데이터는 해밍 거리 측정 방식**을 사용

          ​			

          ​	

     2. Decision Tree(의사결정 트리)

        > 가장 단순한 classifier 중 하나로, decision tree와 같은 도구를 활용하여 모델을 그래프로 그리는 매우 단순한 구조로 되어 있다. 이 방식은 root에서부터 적절한 node를 선택하면서 진행하다가 최종 결정을 내리게 되는 model<span style="font-size:.8em; float: right">(훈련 데이터에 오버피팅이 되는 경향이 있다.)</span>

        중요 개념 

        - 가지치기(pruning) 

        - 엔트로피(Entropy) 

        - 불순도(Impurity)

        - 정보 획득(Information gain)

          

        프로세스

        - 가지치기를 통해 얼만큼의 조건(분기)를 줄지 설정.

        - 엔트로피는 불순도와 같은 의미를 가지는데, 해당 범주안에 서로 다른 데이터가 얼마나 있는지를 뜻.

        - 엔트로피 1는 한 범주안에 정확히 반반의 데이터가 있다는 뜻. 엔트로피 0은 한 범주 안에 하나의 데이터만 있다는 뜻.

        - 즉 불순도가 낮아야 정확한 분류가 되었다는 의미

        - 엔트로피가 1인 상태에서 0.7로 변경되었다면, 정보 획득(Information gain)은 0.3

        - 정보 획득이 최대화하는 방향으로 학습이 진행

          ​			

          ​	

     3. Random Forest

        > Decision tree 여러개 모아 Forest를 이루고 여러 트리의 결과값을 종합해 최종 결과를 판단. <span style="font-size:.8em;">(훈련 데이터 오버피팅을 어느 정도 완화할 수 있다. )</span>

        중요 개념

        - 20개의 feature가 있을 때 이 중 5개의 feature만 가지고 결정트리를 생성 <span style="font-size:.8em; color:grey">(복원 추출/비복원 추출 선택 사용 가능)</span>
        - 또 이 중 5개의 feature만 가지고 결정트리를 생성을 반복하여 결정 트리마다의 예측값들 중 가장 많이 나온 값을 최종 예측값으로 결정

        ​	

        중요 변수

        1. n_estimators: 랜덤 포레스트 안의 결정 트리 갯수 <span style="font-size:.8em">(클수록 좋으나, 그만큼 메모리와 훈련시간이 늘어남)</span>

        2. max_features: 무작위로 선택할 Feature의 개수

           - boostrap = False 설정을 한다면 비복원 추출을 하기 때문에 사실 전체 feature를 사용해 트리를 만듦.

           - boostrap = True 설정은 복원 추출

           - max_features가 크면, 트리들이 비슷하게 생겨 두드러진 특성을 잘 잡고, 작다면 오버피팅이 줄어드는 효과가 있음.

             ​	

             ​		

     4. SVM (Support Vector Machine)

        > 분류와 회귀에서 사용되는 데 분류 모델은 SVC(Support Vector Classifier), 회귀 모델은 SVR(Support Vector Regression)이라 함.
        >
        > 주어진 데이터가 어느 카테고리에 속할지 판단하는 이진 선형 분류 모델
        >
        > **Decision Boundary라는 직선**이 주어진 상태

        중요 인자. kernel, C, Gamma

        1. kernel : decision boundary의 모양을 선형으로 할지 다항식형으로 할지 등을 결정

        2. C : decision boundary의 유연성을 설정. C가 크면 decision boundary는 더 굴곡지고, C가 작으면 decision boundary는 직선에 가깝습니다.

        3. Gamma : reach에 영향을 주어 decision boundary에 가까운(혹은 먼) 요소만을 적용하여 설정. Gamma가 크면 decision boundary는 더 굴곡지고, Gamma가 작으면 decision boundary는 직선에 가깝습니다.

           ​		

           ​	

     5. Naive Bayes (나이브 베이즈)

        > 베이즈 정리에 기반한 통계적 분류 기법. 
        >
        > 나이브 베이즈는 feature끼리 서로 독립이라는 조건이 필요합니다. 즉, 스펨 메일 분류에서 광고성 단어의 개수와 비속어의 개수가 서로 연관이 있어서는 안 됩니다. 

        ​		

        ​	

     6. K-means Clustering(K-평균 클러스터링)

        > 클러스터란 비슷한 특성을 가진 데이터끼리의 묶음입니다. 
        > 즉 K 개의 데이터들의 평균을 활용한 군집(Cluster)을 만들고 이에 따른 분류를 하는 기술입니다.

        중요 개념

        - **클러스터의 중심(means)을 Centroid**라고 합니다.

          ​	

        프로세스

        1. 몇 개의 클러스터(class)가 필요한지 결정 (K 결정)

        2. 초기 Centroid 위치 지정
           - 랜덤하게 지정
           - 수동으로 지정
           - Kmean ++ 지정

        3. 모든 데이터를 순회하며 각 데이터마다 가장 가까운 Centroid가 속해있는 class로 할당

        4. 할당된 class 별 Centroid의 평균(means)으로 Centroid를 이동

        5. 클러스터에 할당되는 데이터에 변화가 없을 때까지 스텝 3, 4를 반복

        ​		

2. 회귀(regression)

   > 어떤 정확한 값을 예측하는 경우에 사용합니다.

   - 집 평수에 따른 이사 비용 예측, 가전기기 수에 따른 전력량 예측 등

     모델 종류

     1. Gaussian Processes(GP)

        - 예측값의 신뢰구간 혹은 비신뢰구간을 알 수 있다. 

        - 추세를 나타내기에 좋다. 

          

     2. 

         

   

3. 예측(Forecast)

   > 어떤 값을 예측하기 위해서는 기존의 값들을 알아야 한다. 즉 레이블 작업이 필요.

   - 회귀 + <b>시간</b>
   - 다음달 기름값 가격, 다음달 GPU 가격 등
     - arima
     - GP
     - SVR

​	

4. 이상값 감지(Anomaly Detection, 비정상값 발견)
   - 특정 패턴 혹은, 유형을 벗어난 값을 감지

​			

5. 그룹화(Clustering)

   > 주어진 데이터들을 그룹으로 묶기위헤서는 그 특성을 대조하면 된다. 즉 레이블 작업 불필요.

   - 전체 데이터를 주면 특징을 기준으로 그룹해주는 경우
   - 분류와의 차이점 : 분류는 정해진 카테고리 안에서 어디에 속하는지 확인
   - 

​	

​	

6. 강화학습(Reinforcement Learning)

   > 목표 지향 학습을 통해 진행.

   - 특정 케이스에 집중해서 컴퓨터를 학습시키는 경우



​	

​	

​	

​	

​	



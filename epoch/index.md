# Epoch


​	

# Epoch의 의미

1~100의 데이터가 input의 양(x_train)으로 주어졌을 때 

20 epoch를 적용한다는 의미는



단계별로 설명하자면, 

1. 1번째 학습(1 epoch) 시작

2. 초기화된 가중치(w)와 편향(b)을 가지고 1에 대한 계산 진행 > 

3. 1을 대입하여 나온 가중치(w) 와 편향(b) 도출 

4. 2에게 1을 대입하여 나온 가중치(w) 와 편향(b)을 대입하여 새로운 가중치(w)와 편향(b)가 도출

5. 3에게 2를 대입하여 나온 가중치(w) 와 편향(b)을 대입하여 새로운 가중치(w)와 편향(b)가 도출

6.  ...

7. 100에게 99를 대입하여 나온 가중치(w) 와 편향(b)을 대입하여 새로운 가중치(w)와 편향(b)가 도출

1 Epoch 학습 종료



2번째 학습(2 epoch) 시작

1. 1 Epoch를 통해 마지막으로 도출된 가중치(w)와 편향(b)을 가지고 다시 1에 대한 계산 진행

2. 1을 대입하여 나온 가중치(w) 와 편향(b) 도출 

3. 2에게 1을 대입하여 나온 가중치(w) 와 편향(b)을 대입하여 새로운 가중치(w)와 편향(b)가 도출

4. 3에게 2를 대입하여 나온 가중치(w) 와 편향(b)을 대입하여 새로운 가중치(w)와 편향(b)가 도출

5. ...

6. 100에게 99를 대입하여 나온 가중치(w) 와 편향(b)을 대입하여 새로운 가중치(w)와 편향(b)가 도출



...반복

​		

과 같이 

1번의 데이터 전체를 학습하고 난 후 도출된 가중치(w)와 편향(b) 계산하는 과정을 Epoch하고 합니다.

​	

​	

​	

​	

​	

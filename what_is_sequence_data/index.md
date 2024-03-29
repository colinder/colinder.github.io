# What is Sequence_data & apply AI


​	

# Sequence data

자연에는 사건이 발생하는데, 이는 대부분 시간에 따른 원인에 의한 결과로써 발생하게 된다. 예를들어 

1. 오전에는 맑았는데 오후에는 흐리다.

2. 상반기에는 잘 팔렸는데, 하반기에는 잘 안팔린다.

3. 기분이 나빴는데, 좋아졌다.  등등

>  순서(sequence)대로 하나씩 나열하여 나타낼 수 있는 데이터를 `Sequence data`라고 한다.
>
> 그리고 이 `데이터들은 서로 독립적이지 않다.`는 특징을 가지고 있다.  즉,  원인과 결과가 무한이 이어져있다.

---

​	

​	

### Sequence data의 구조와 주로 활용되는 형태

많은 사람들이 시도해보는 주식 예측 인공지능 개발을 가정하여 설명.

- A회사의 '주가'를 예측할 것이며 주식은 시간에 따라 ㄱ, ㄴ, ㄷ, ㄹ, ㅁ이라는 사건에 영향을 받는다.
- ㄱ, ㄴ, ㄷ, ㄹ, ㅁ은 수치화 할 수 있다. 

​			

A회사 주식의 변화를 표로 나다내면 아래와 같다. 

| 날짜       | 주가   | ㄱ   | ㄴ   | ㄷ   | ㄹ   | ㅁ   |
| ---------- | ------ | ---- | ---- | ---- | ---- | ---- |
| 1900-01-01 | 10,000 | 45   | 6    | 23   | 54   | 3    |
| 1900-01-02 | 10,100 | 41   | 8    | 28   | 50   | 1    |
| 1900-01-04 | 10,200 | 39   | 4    | 30   | 47   | 2    |
| 1900-01-05 | 10,200 | 36   | 4    | 26   | 49   | 3    |
| 1900-01-06 | 10,500 | 47   | 7    | 29   | 46   | 1    |
| 1900-01-07 | 10,100 | 50   | 10   | 24   | 44   | 2    |
| 1900-01-08 | 10,200 | 60   | 8    | 25   | 50   | 3    |

**우선 간단히 데이터를 분석하자면,**

1. 1900-01-01부터 보통 1일간의 간격(sequence)으로 데이터가 수집되었다. 
2. 1900-01-04 데이터는 결측되었다. 
3. 데이터가 수집된 일자의 변수(feature) ㄱ, ㄴ, ㄷ, ㄹ, ㅁ의 결측은 없다. 

정도로 간단히 분석할 수 있다. 

​	

​	

### Apply AI

**이를 인공지능에 적용하기 위해서는 `데이터 전처리 `라는 과정이 필요하다.**

데이터를 어떻게 전처리해 AI의 학습 데이터로 사용할지는 

1. 전처리 방법의 차이 
2. 학습용 데이터 구조를 어떻게 만드느냐?
3. 어떤 모델을 사용할 것이냐?

 등의 차이가 있고, 절대적인 `정답`은 없다.

​	

**간단히 시퀀스 데이터 형태로 변환하자면,**

data_set = [[45,6,23,54,3], [41,8,28,50,1], [39,4,30,47,2], [36,4,26,49,3], [47,7,29,46,1], [50,10,24,44,2], [60,8,25,50,3]]

와 같이 구조화할 수 있겠다. 

---

​	



​	



#### 💡 여기서 알아야할 점이 있다. 아래와 같은 데이터가 있다.

**seq_data = [0, 3, 4, 5]**

**filter = [1, 2]**

**seq_data**에 **filter**를 곱해(**seq_data** ⦿ **filter**)

<image src="../images/what_is_sequence_data.assets\Screen-Shot-2017-12-21-at-17.29.53-300x251.png" style="display: block; margin: 10px auto;">

<image src="../images/what_is_sequence_data.assets\image-20221025133232909.png" style="display: block; margin: 10px auto;">

<b>output = [6, 11, 14]</b> 라는 결과가 나왔다. 

​	

*해당 과정의 흐름을 기억해야 한다.     /&nbsp;&nbsp; **seq_data**에 **filter**를 적용해 **output**을 계산했다.

#### (**seq_data** ⦿ **filter** = **output**)

​	

위의 주식 예측 인공지능에 맞대어 생각해본다면, 

- [0, 3]라는 변수가 있을 때 주가가 6이었고,
- [3, 4]라는 변수가 있을 때 주가가 11이었고,
- [4, 5]라는 변수가 있을 때 주가가 14이었다.

​	

​	

<b>그렇다면, 주식 예측 인공지능를 개발하기 위해서 진행되어야 하는 과정은</b>

**seq_data**가 있고 **output**이 있을 때, **filter** 값를 찾아 **향후 seq_data**에 대입해 계산해보는 것이다. 

즉, (**seq_data** ⦿ **<span style="color:red">filter</span>** = **output**)에서 **<span style="color:red">filter</span>** 값을 찾아주는 것이 인공지능의 역할이며

위의 **filter**라는 명칭은 인공지능에서 <b>가중치(weight)</b>라고 불리며 이를 찾아 가는 것이 인공지능의 핵심이다.

​	

​	

​	

아까 주식 데이터에서 날짜를 고려하지 않고, 3일간의 데이터를 사용해 1일 뒤<span style="color: grey; font-size: .8em">(1900-01-09)</span>의 주가를 예측한다고 했을 때,

| 날짜       | 주가   | ㄱ   | ㄴ   | ㄷ   | ㄹ   | ㅁ   |
| ---------- | ------ | ---- | ---- | ---- | ---- | ---- |
| 1900-01-01 | 10,000 | 45   | 6    | 23   | 54   | 3    |
| 1900-01-02 | 10,100 | 41   | 8    | 28   | 50   | 1    |
| 1900-01-04 | 10,200 | 39   | 4    | 30   | 47   | 2    |
| 1900-01-05 | 10,200 | 36   | 4    | 26   | 49   | 3    |
| 1900-01-06 | 10,500 | 47   | 7    | 29   | 46   | 1    |
| 1900-01-07 | 10,100 | 50   | 10   | 24   | 44   | 2    |
| 1900-01-08 | 10,200 | 60   | 8    | 25   | 50   | 3    |

01-01, 01-02 데이터는 활용하기 어렵다. (별도의 결측치 대치 전처리를 하지 않는 이상.)

=> 연속된 4일간의 데이터가 있어야, 주제에 맞는 인공지능 학습용 데이터 세트를 만들 수 있다.

​	

[[39,4,30,47,2], [36,4,26,49,3], [47,7,29,46,1]] x  <b>filter(weight)</b>  = 10,100

[[36,4,26,49,3], [47,7,29,46,1], [50,10,24,44,2]] x  <b>filter(weight)</b>  = 10,200

[[47,7,29,46,1], [50,10,24,44,2], [60,8,25,50,3]] x  <b>filter(weight)</b>  = ?

이렇게 인공지능은 <b>filter(weight)</b>를 찾아 "예측"을 하게 된다.  

​	

​	

​	

​	

​	

이 과정에 "예측"에 대한 많은 오점과 오류와 가정를 찾기를 바란다. 

이와 같은 방법이 맞을 수 있으나, 우리가 사는 세상에 적용하기에 수많은 오점과 오류가 있으며, 가정이 필요하다.

​	

​	

​	

​	


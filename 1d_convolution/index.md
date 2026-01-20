<script src="/livereload.js?mindelay=10&amp;v=2&amp;port=1313&amp;path=livereload" data-no-instant defer></script># 1D_convolution


​	

# 1D convolution

> 합성곱*의 개념을 도입해 시계열의 특성을 가지는 데이터의 특성을 추출하는 딥러닝 모델.
>
> 1차원 CNN은 이미지가 아닌 시계열 분석 (time-series analysis)나 텍스트 분석을 하는데 주로 많이 사용된다. 여기에서 1차원이라는 것은 합성곱을 위한 커널과 적용하는 데이터의 sequence가 1차원의 모양을 가진다는 것을 의미.

​	

​	

### 합성곱이란?

- `이론적으로`, 하나의 함수와 또 다른 함수를 반전 이동한 값을 곱한 다음, 구간에 대해 적분하여 새로운 함수를 구하는 수학 연산자.

- `AI에서는 주로`, 합성곱 연산이란 커널(kernel) 또는 필터(filter) 라는 n × m 크기의 행렬로 높이(height) × 너비(width) 크기의 이미지(혹은 시계열성 데이터)를 처음부터 끝까지 겹치며 훑으면서 n × m 크기의 겹쳐지는 부분의 각 이미지와 커널의 원소의 값을 곱해서 모두 더한 값을 출력으로 하는 것을 의미. 이때, 왼쪽 위부터 가장 오른쪽 아래까지 순차적으로 훑습니다.

​	

​	

#### pytorch를 활용한 1D conv 예제

```python
import torch.nn as nn

conv1 = nn.Conv1d(in_channels=4, out_channels=64, kernel_size=5, 
                  stride=3, padding=0, dilation=1, 
                  groups=1, bias=True, padding_mode='zeros')

# default setting
# you have to set in_channels, output_channels and kernel_size
conv2 = nn.Conv1d(in_channels=4, out_channels=64, kernel_size=5)
```

​	

#### **Parameters**

- in_channels: input의 feature dimension
- out_channels: 내가 output으로 내고싶은 dimension
- kernel_size: time step을 얼마만큼 볼 것인가(=frame size = filter size)
- stride: kernel을 얼마만큼씩 이동하면서 적용할 것인가 (Default: 1) -> 아래 추가 설명
- dilation: kernel 내부에서 얼마만큼 띄어서 kernel을 적용할 것인가 (Default: 1) -> 아래 추가 설명
- padding: 한 쪽 방향으로 얼마만큼 padding할 것인가 (그 만큼 양방향으로 적용) (Default: 0)
- groups: kernel의 height를 조절 (Default: 1) -> 아래 추가 설명
- bias: bias term을 둘 것인가 안둘 것인가 (Default: True)
- padding_mode: 'zeros', 'reflect', 'reflect', 'replicate', 'circular' (Default: 'zeros') 

​	

​	

#### **Input**

input은 (Batch, `Feature dimension`, `Time_step`)순이다. 즉 `Time step(K)` 마다 `Feature dimension(N)`이 존재하는 2차원 배열이 input의 형태. <span style="color: grey; font-size:0.8em">(batch size(B)까지 고려하면 3차원이지만 알아서 계산해주므로 아래 그림에선 생략)</span>

<image src="../images/1D_convolution.assets/img.png" style="display: block; margin: 10px auto;">

​	

#### **Operation**

1차원 시계열 데이터의 경우 (`Feature dimension`, `Time_step`) 이렇게 2개의 차원이 존재하므로 두 번째 차원인 `Feature dimension`이 채널(channel)이 된다. 그래서 in_channels은 위의 예시에서 N이 된다. 여기에 기본적인 1D convolution을 적용해보자.

- 1D convolution은 width가 kernel size가 되고 length는 input channel과 같아진다. 그리고 `kernel이 out channel만큼 생성된다.` 아래의 예시를 보면, `out channel=3`일 때 3개의 커널을 이용해<span style="color: grey; font-size: .8em">(각각 주황, 초록, 파랑색으로 커널을 구분)</span>을 이용해 convolution 연산을 진행하고 3개의 결과를 각각 도출한다. 

  <image src="../images/1D_convolution.assets/img-16661664654091.png" style="display: block; margin: 10px auto;">

​	

​	

#### **Output**

output은 (Batch, feature dimension, kernel로 변경된 time_step) 순이다. output은 아래의 그림으로 표현할 수 있다.

<image src="../images/1D_convolution.assets/img-16661664654092.png" style="display: block; margin: 10px auto;">















### 참고 사이트

- https://sanghyu.tistory.com/24

- http://www.jussihuotari.com/2017/12/20/spell-out-convolution-1d-in-cnns/


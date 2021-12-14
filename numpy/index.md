# Numpy


​	

# Numpy

> ML, AI 등 개발을 진행하다보면 무지성으로 import하는 library가 있다. 
>
> **import numpy as np**
>
> python만 할 줄 안다면 어렵지 않게 사용이 가능하지만, 그래도 numpy의 개념을 알고 진행하고 싶은 마음에 내용을 정리해보자

​	

## What is <u>Numpy</u>(Numerical Python)?

> 공식문서를 따르면,<br>
> NumPy is the fundamental package for scientific computing in Python. It is a Python library that provides a multidimensional array object, various derived objects (such as masked arrays and matrices), and an assortment of routines for fast operations on arrays, including mathematical, logical, shape manipulation, sorting, selecting, I/O, discrete Fourier transforms, basic linear algebra, basic statistical operations, random simulation and much more.

1. NumPy는 python의 과학 컴퓨팅(scientific computing)을 위한 기본 패키지

   - *scientific computing: **과학** 및 **공학** 문제의 수학적 모델을 컴퓨터로 푸는 데 필요한 도구, 기술 및 이론의 모음*

2. '다차원 배열' 여기서 '파생되는 객체' 및 '수학', '논리', '기본 선형대수' 등을 지원하는 python library

   ​	

> At the core of the NumPy package, is the *ndarray* object. This encapsulates *n*-dimensional arrays of homogeneous data types, with many operations being performed in compiled code for performance.

- Numpy의 핵심은 ndarray 객체이며, 이는

- 코드의 빠른 처리를 위해 **동일 데이터 타입**의 n차원 배열을 캡슐화한다. == 하나의 데이터 타입만 배열에 넣을 수 있다. 

  <image src="/images/numpy.assets/image-20211214131438955.png" width="1000px" style="display: block;">

  ​	

### 특징

- 일반 리스트에 비해 빠르고 효율적

- 반복문 없이 데이터 배열에 대한 처리 지원

- 선형대수와 관련된 다양한 기능을 제공

  ​		

  ​	

## Scalar ?

> Scalar는 **0차원 데이터**

<image src="/images/numpy.assets/image-20211214140958930.png" width="800px" style="display: block;">

​	

​	

​	

## Vector ?

>Vector는 **1차원 데이터**

<image src="/images/numpy.assets/image-20211214140922463.png" width="800px" style="display: block;">

​	

---

```python
>>> import numpy as np
>>> a = np.array([1, 2, 3])
```

다음과 같이 배열을 시각화할 수 있습니다.

![../_이미지/np_array.png](https://numpy.org/doc/stable/_images/np_array.png)

#### 여기서 잠깐! ✋

<image src="/images/numpy.assets/image-20211214133421746.png" width="800px" style="display: block;">

- d는 <b>5행 1열</b>이 맞습니다. *다만 출력된 화면은 <b>1행 5열</b>로 보입니다.*
- e는 <b>2행 5열</b>이 맞습니다. 출력된 화면대로 이해하면 됩니다.

​		

​		

​		

## Matrix ?

> Matrix는 <b>2차원(행, 열) 데이터</b>

<image src="/images/numpy.assets/image-20211214141907385.png" width="800px">

​		

​		

​		

## Tensor ?

> Tensor는 <b>3차원 이상의 다차원 행렬 데이터</b>

<image src="/images/numpy.assets/image-20211214142202572.png" width="800px">

​	

​	

​	

​	



# 'NAVER AI NOW' HyperCLOVA_NLP


# NAVER HyperCLOVA의 한국어 모델

> 2021.05.25 NAVER AI NOW
> HyperCLOVA의 자연어 전처리 과정의 **토근화** 방법에 대하여 정리

​	

## 데이터 토큰화

> 자연어 처리를 위한 문장 데이터 토큰화에 대한 NAVER의 처리 방법을 정리해보겠습니다.
>
> - 말뭉치를 어떻게 구성하고 나눌 것인가에 대한 고민 → **서브워드 토크나이저**를 활용. 
>
> - 서브워드는 어떤방식으로 진행할 것인가? → <b>Byte-Pair Encoding (BPE)</b>를 사용<br>
>   그 중에서도 <b>Morpheme-Aware Byte-Level Byte Pair Encoding(형태소 단위로 분리)</b>을 사용
>
> - Morpheme-Aware Byte-Level Byte Pair Encoding는 많은 메모리를 필요로 하여 말뭉치 데이터의 크기를 최적화하는 것이 필요함.

​	  	

## 서브워드란?

자연어 처리를 하려면 기계가 문장을 끊어 읽을 수 있게 만들어줘야 합니다. 이를 보통 토크나이징(tokenizing)이라고 하며 나눠진 단어나 형태소를 **토큰**이라고 합니다. 하지만 토큰은 '문장'에서 추출한 데이터고 / '문장'은 작성한 사람에 따라 구성이 다르기도 하며, 신조어를 사용하는 등 고려해야하는 사항이 많습니다. 특히 Out of Vocabulary(OOV) 미등록 된 단어의 문제는 자연어 처리에 있어서 해결이 어려운 이슈사항입니다.

그리고 이를 보완하기 위해 **Subword** 방식이 대두되었습니다. 
**하나의 단어는 더 작은 단위의 의미있는 여러 서브워드들의 조합으로 구성된 경우가 많기 때문**에, 하나의 단어를 여러 서브워드로 분리해서 기계어로 변환하기 위한 작업입니다. 

​		

## BPE(Byte Pair Encoding)

BPE은 **압축 알고리즘**으로 연속적으로 가장 많이 등장한 글자의 쌍을 찾아서 하나의 글자로 병합하는 방식을 수행합니다. 

```
# dictionary
l o w : 5,  l o w e r : 2,  n e w e s t : 6,  w i d e s t : 3
```

딕셔너리를 참고로 한 초기 단어 집합(vocabulary)을 아래와 같습니다. 간단히 말해 **초기 구성은 글자 단위로 분리된 상태입니다.**

```
# vocabulary
l, o, w, e, r, n, w, s, t, i, d
```

BPE의 특징은 알고리즘의 동작을 몇 회 반복(iteration)할 것인지를 사용자가 정한다는 점입니다. 여기서는 총 10회를 수행한다고 가정합니다. 다시 말해 **가장 빈도수가 높은 유니그램의 쌍을 하나의 유니그램으로 통합**하는 과정을 총 10회 반복합니다. 위의 딕셔너리에 따르면 빈도수가 현재 가장 높은 유니그램의 쌍은 (e, s)입니다.

**1회 - 딕셔너리를 참고로 하였을 때 빈도수가 9로 가장 높은 (e, s)의 쌍을 es로 통합합니다.**

```
# dictionary update!
l o w : 5,
l o w e r : 2,
n e w es t : 6,
w i d es t : 3
# vocabulary update!
l, o, w, e, r, n, w, s, t, i, d, es
```

**2회 - 빈도수가 9로 가장 높은 (es, t)의 쌍을 est로 통합합니다.**

```
# dictionary update!
l o w : 5,
l o w e r : 2,
n e w est : 6,
w i d est : 3
# vocabulary update!
l, o, w, e, r, n, w, s, t, i, d, es, est
```

**3회 - 빈도수가 7로 가장 높은 (l, o)의 쌍을 lo로 통합합니다.**

```
# dictionary update!
lo w : 5,
lo w e r : 2,
n e w est : 6,
w i d est : 3
# vocabulary update!
l, o, w, e, r, n, w, s, t, i, d, es, est, lo
```

이와 같은 방식으로 총 10회 반복하였을 때 얻은 딕셔너리와 단어 집합은 아래와 같습니다.

```
# dictionary update!
low : 5,
low e r : 2,
newest : 6,
widest : 3
# vocabulary update!
l, o, w, e, r, n, w, s, t, i, d, es, est, lo, low, ne, new, newest, wi, wid, widest
```

<p style="text-align:right; color:grey; font-style:italic; font-size:0.9em">출처  https://wikidocs.net/22592</p>

​		

## Morpheme-Aware Byte-Level Byte Pair Encoding

Byte Pair Encoding을 한국어를 형태소 수준으로 나누어 진행
ex) '나는 배가고프다.' → '나', '는', '배', '가', '고', '프', '다'. 로 	나누어 Byte Pair Encoding을 진행

​	

## 서브워드로 나눈 말뭉치의 크기는 얼마나 설정할 것인가?

1. 일반적으로 자연어 말뭉치에 사용된 단어를 빈도순으로 나열했을 경우 <b>지프의 법칙(Zipf's Law)</b>을 따름.

   > Zipf's Law :  어떠한 [자연어](https://ko.wikipedia.org/wiki/자연어) [말뭉치](https://ko.wikipedia.org/wiki/말뭉치) 표현에 나타나는 단어들을 그 사용 빈도가 높은 순서대로 나열하였을 때, 모든 단어의 사용 빈도는 해당 단어의 순위에 [반비례](https://ko.wikipedia.org/wiki/반비례)한다. 따라서 가장 사용 빈도가 높은 단어는 두 번째 단어보다 빈도가 약 두 배 높으며, 세 번째 단어보다는 빈도가 세 배 높다.
   >
   > 한가지 예로 )_약 135개 항목의 어휘만으로 브라운 대학 말뭉치의 절반을 나타낼 수 있다._

2. 1에 따라 **네이버는 _학습에 사용한 말뭉치 양의 차이에 따른 서브워드 토크나이저의 어위 집합(Vocabulary)에는 큰 차이가 없을 것으로 판단._**

👉  네이버의 경우, 전체 말뭉치와 전체 말뭉치의 1% (20GB)로 서브워드 토크나이저를 실행한 결과를 비교해보니 어휘 집합 구성의 유사함을 확인.

 			

## 성능 평가 - 언어 생성관점

NAVER는 발전한 대규모 모델의 언어생성능력에 비해 기존의 평가 지표(품질: BLEU, ROUGE, METEOR... / 다양성: Self-BLEU, Distinct-N, Unique-N...)들이 여전이 2000년대 초반에 머물러 있다는 점을 파악 

1. '생성 문장'과 '레퍼런스 문장'의 유사성이 더 이상 문장의 품질을 보장하지 않는다.  

2. 학습 어휘에 따라 성능에 차이가 발생함으로 perplexity(ppL)로 비교하는 것은 부적절하다.

   > 펄플렉서티(perplexity)는 언어 모델을 평가하기 위한 내부 평가 지표. 'perplexed'는 '헷갈리는'과 유사한 의미를 가집니다. 그러니까 여기서 PPL은 '헷갈리는 정도'로 레퍼런스들과의 차이를 판별하는 지표로 이해하면 됩니다. 

👉  네이버의 경우, '언어의 유창함'을 판별활 수 있는 모델을 만들어 자체적으로 성능을 평가함.

​			

​			

# 👀요약

> NLP관련 자료들을 보다 보니 개인적으로 고집해봤던 '완벽한 토큰화'는 아직 불가능해 보였습니다. konlpy, 한국전자통신연구원(ETRI), google NLP 등...
> 다만, 이번에 개발된 네이버의 자연어 처리기술이 한국어에 있어서는 가장 진보되었고 '사람다운' 처리를 보여주는 것 같습니다.


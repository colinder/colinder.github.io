# Software License


​			

# License

> 실무를 담당하고 있다면, 새로운 프로젝트에 앞서 어떤 기술을 사용할 것인지 조사하는 것도 업무중의 하나입니다. 
>
> 근데 항상 고민이 되는 부분. **비용**과 **권한**
>
> 그리고 비용과 권한 정리되어 있는 것이 **License**

​			

[한국저작권위원회의 OLIS(오픈소스SW 라이선스 종합정보시스템)](https://www.olis.or.kr/license/introduction.do)에는 라이선스에 대한 내용이 잘 정리 되어 있습니다. 많은 라이선스들이 있지만,<br>
접해본 라이선스 위주로, 개발을 위해 중요하게 보았던 부분을 정리합니다.

​		

​		

## 1. MIT License

> 아마 가장 흔하게 접하는 라이선스일 것입니다.
>
> "MIT 라이선스(MIT License)는 미국 매사추세츠 공과대학교(MIT)에서 해당 대학의 소프트웨어 공학도들을 돕기 위해 개발한 라이선스다. MIT 라이선스를 따르는 소프트웨어를 개조한 제품을 반드시 오픈 소스로 배포해야 한다는 규정이 **없으며** GNU 일반 공중 라이선스의 엄격함을 피하려는 사용자들에게 인기가 있다."

​		

주요 특징

1. 복제, 배포, 수정의 권한 허용: 가능

2. 배포시 라이선스 사본 첨부: 가능

3. 배포시 소스코드 제공의무(Reciprocity)와 범위: 명시되어 있지 않음

4. 조합저작물(Lager Work) 작성 및 타 라이선스 배포 허용: 가능

5. 책임의 제한: 가능

   ​		


MIT license를 사용한 프로젝트 배포시 의무사항

- 저작권 안내문구, MIT 라이선스 문구가 모든 복제본에 포함

  ​	

  ​	

## 2. GPL(GNU) License - 일반 공중 사용 허가서

> "자유나 죽음이냐" 완전한 자유를 꿈꾸는 라이선스입니다. 
> 자유에는 제한이 없으며, 모든 것이 공유되어야하며, 책임이 따른다. 는 관점의 라이선스입니다.
>
> "자유 소프트웨어 재단(FSF)에서 만든 자유 소프트웨어 라이선스다. 미국의 리처드 스톨만(Richard Stallman)이 GNU-프로젝트로 배포된 프로그램의 라이선스로 사용하기 위해 작성했다. '① 컴퓨터 프로그램을 어떤 목적으로든지 사용할 수 있다 ② 컴퓨터 프로그램의 복사를 언제나 프로그램의 코드와 함께 판매 또는 무료로 배포할 수 있다 ③ 컴퓨터 프로그램의 코드를 용도에 따라 결정할 수 있다 ④ 변경된 컴퓨터 프로그램 역시 프로그램의 코드와 함께 자유로이 배포할 수 있다'라는 네 가지 조항을 명시하고 있다."
>
> "대부분의 소프트웨어에 대한 라이선스는 소프트웨어를 공유하거나 수정할 수 있는 자유를 금지하기 위 고안되었다. 반면에 GNU 일반 공중 라이선스는 자유 소프트웨어를 공유하고 수정할 수 있는 자유를 보장하기 위해 의도되었다. 즉, 소프트웨어가 사용자 모두에게 자유롭게 이용될 수 있도록 하는 것이다. 이 일반 공중 라이선스는 자유 소프트웨어 재단의 소프트웨어 대부분을 비롯하여, 저작자가 이 라이선스의 사용을 지정한 기타 모든 프로그램에 적용된다. (자유 소프트웨어 재단의 소프트웨어 중 일부는 이 라이선스 대신 GNU 라이브러리 일반 공중 라이선스가 적용된다.) 누구나 자신의 프로그램에 이 라이선스를 적용시킬 수 있다."

​			

주요 특징

1. 복제, 배포, 수정의 권한 허용: 가능

2. 배포시 라이선스 사본 첨부: 가능

3. 배포시 소스코드 제공의무(Reciprocity)와 범위: `WORK BASED ON THE CODE`

   - `WORK BASED ON THE CODE`

     <span style="font-size:.9em">제공의무: 원 저작물의 소스코드를 원본 그대로, 혹은 수정하여 새로운 SW에 포함하였을 경우 공개</span><br>
     <span style="font-size:.9em">제공범위: 원 저작물의 소스코드가 포함되어, 파생 저작물로 인정되는 범위내의 모든 소스코드 공개</span>

4. 조합저작물(Lager Work) 작성 및 타 라이선스 배포 허용: 조건부 가능

   - 보통의 경우 상업용 라이선스가 별도로 존재하는 것 같습니다. (비용 ㅎㄷㄷ...)

5. 명시적 특허 라이선스의 허용: 명시되어 있지 않음.

   ​	

GPL(GNU) license를 사용한 프로젝트 배포시 의무사항

- 각 복제본에 적절한 저작권 고지와 보증책임이 없음을 명시

- GPL 라이선스를 언급하는 고지사항과 보증책임 관련 고지사항을 원본 그대로 유지

- 프로그램을 양도 받는 모든 이들에게 프로그램과 함께 GPL 라이선스 사본 제공

- 파일 수정의 경우 수정사실과 날짜를 파일에 명기

- 원본저작물과 파생저작물을 GPL 2.0에 의해 배포

- 원본저작물 및 파생저작물에 대한 소스코드를 제공하거나, 요청시 제공하겠다는 약정서 제공

  ​	

**간단히, 프로젝트의 코드가 공개되지 않아야 한다면, 비용없이 개발하고 싶다면 이 License의 오픈소스는 사용하면 안됩니다.**

​	

​	



## 3. LGPL3 (Lesser General Public License version 3.0)

>[LGPL](https://www.olis.or.kr/license/Detailselect.do?lId=1073&mapCode=010073&lType=osi)은 GPL보다는 훨씬 완화된(Lesser) 조건의 공개 소프트웨어 라이선스입니다.
>
>라이브러리는 공유하되 개발된 제품에 대해서는 소스를 공개하지 않고 상용 SW 판매가 가능한 GPL 보다 완화된 라이선스를 말함.
>
>가장 큰 차이점은 LGPL 코드를 정적(static) 또는 동적(dynamic) 라이브러리로 사용한 프로그램을 개발하여 판매/배포할 경우에 프로그램의 소스코드를 공개하지 않아도 된다는 점입니다. LGPL 코드를 사용했음을 명시만 하면 됩니다.
>
>**단, LGPL 코드를 단순히 이용하는 것이 아니라 이를 수정한 또는 이로부터 파생된 라이브러리를 개발하여 배포하는 경우에는 전체 코드를 공개해야 합니다.**

​		

주요 특징

1. 복제, 배포, 수정의 권한 허용: 가능

2. 배포시 라이선스 사본 첨부: 가능

3. 배포시 소스코드 제공의무(Reciprocity)와 범위: `DERIVATIVE WORK (파생작업물)`

   - `DERIVATIVE WORK`

       <span style="font-size:.9em">제공의무: 원 저작물의 소스코드를 수정하여 사용한 경우 제공의무가 존재하며, 수정 없이 그대로
        사용하였을 경우에는 소스코드를 제공하지 않아도 됨</span><br>
       <span style="font-size:.9em">제공범위: 원 저작물을 사용함에 있어 수정을 거쳤다면, 원 저작물의 소스코드에서부터 존재하던
        파일을 모두 공개해야 하며, 파생 저작물의 저작자가 추가적으로 생성한 부분에 대해서는 공개하지
        않아도 됨</span>

4. 조합저작물(Lager Work) 작성 및 타 라이선스 배포 허용: 가능

5. 이름, 상표, 상호에 대한 사용제한: 명시되어있지 않음.

   ​		

LGPLv3 license를 사용한 프로젝트 배포시 의무사항

- 각 복제본에 저작권 고지와 보증책임이 없음을 명시
- LGPL 3.0의 조건 및 제7조의 조건에 관한 내용을 있는 그대로 유지
- 프로그램을 양도 받는 모든 이들에게 프로그램과 함께 GPL 및 LGPL 라이선스 사본 제공
- 수정시 수정사실 및 일시를 명시
- 원본저작물과 파생저작물을 LGPL3.0에 의해 배포
- 원본저작물 및 파생저작물에 대한 소스코드를 제공하거나, 요청시 제공하겠다는 약정서 제공
- 사용자제품에 대한 인증키 등 설치정보의 제공
- 응용프로그램을 배포할 경우, LGPL 라이브러리를 사용하고 있다는 사실을 명시
- 사용자가 라이브러리를 수정해도 응용프로그램을 사용할 수 있도록 (예를 들어 오브젝트코드 등을 제공하거나 공유라이브러리 방식등을 이용하여) 허용

  ​			

**라이브러리 소스코드를 수정 또는 파생하여 사용한 경우에는 공개의무가 발생하지만, 그렇지 않으면 상업용으로 사용해도 공개 의무가 없습니다.**

**또 정적링크(ex. .exe 파일)로 개발하여 사용 시 수정코드를 포함한 라이브러리의 소스코드와 응용프로그램의 오브젝트 코드에 대한 공개의무가 발생합니다.**

​		

​		

​			

## ✨라이브러리마다 자체의 라이센스 내용이 추가되는 경우가 많습니다. 라이브러리 제작자(제공자) 자체 라이센스 내용도 확인해야 합니다.

​	

​	

​	

​	

​	

​	

​	

### 정보통신산업진흥원에서는 `공개SW 라이선스`를 아래와 같이 정리하고 있습니다. 

> **공개SW(Open Source Software)는 소스가 공개되어 사용, 복제, 배포,수정 할 수 있지만, 원 저작자의 라이선스 규칙에 따라야 함**
>
> 공개SW(Open Source Software) 라이선스란 공개SW 개발자와 이용자 간의 사용 방법 및 조건의 범위를 명시한 계약을 말한다.
> 따라서 공개SW를 이용하려면 공개SW 개발자가 만들어놓은 조건의 범위에 따라 해당 소프트웨어를 사용해야 하며, 이를 위반할 경우에는 라이선스 위반 및 저작권 침해로 이에 대한 법적 책임을 져야한다.

​	

​	

<image src="/images/license_00.png" width="auto" style="display: block; margin: 10px auto;">

​		

​		

​			

<image src="/images/license_01.png" width="auto" style="display: block; margin: 10px auto;">

​	

​	

​	


# Process VS Thread


​	

# <span style="color: #358691">Process</span> VS <span style="color: #dabc9a">Thread</span>

> 면접 질문에서 가장 많이 들어본 이야기라고 친구들이 이야기 해줬습니다. 근데 전 아직 자신있게 설명할 정도로 알지 못하기에 알아봅시다!

​	

​	

## 프로그램(Program)란 무엇인지 우선 알아봅시다.

소프트웨어의 한가지로, 어떤 문제를 해결하기 위하여 그 처리 방법과 순서(a.k.a  ALGORITHM)를 기술하여 컴퓨터에 주어지는 일련의 명령문 집합체를 뜻합니다. 쉽게 말해,  “어떤 작업을 위해 실행할 수 있는 파일”을 뜻합니다. 그리고 그 프로그램을 실행 시키는 주체를 <b>인스턴스</b>라고도 표현합니다.

예들 들어, Excel.exe, kakaotalk.exe 등과 같이 어떤 작업을 위해 실행할 수 있는 (설치 등)파일을 생각해 볼 수 있습니다.

​	

​	

# <span style="color: #358691">Process</span>

Process는 `프로그램의 인스턴스(독립적인 개체)가 실행되어 메모리에 적재된 것`을 말합니다. 조금 다르게 `운영체제로부터 시스템 자원을 할당받는 자원의 크기`라고도 말할 수 있습니다. 조금 간단하게는 "실행된 프로그램을 의미"합니다.

앞서 운영체제로부터 할당을 받는다고 설명했는데 <b>CPU의 시간</b>  /  **운영되기 위해 필요한 주소 공간**  /  **Code, Data, Stack, Heap의 구조로 되어 있는 독립된 메모리 영역**등이 대상입니다.

<image src="../images/ProcessVSThread_00.png" width="700px" style="display: block; margin-left: auto; margin-right: auto; margin-top: 20px">

​	

즉, **프로그램을 실행하게 되면 CPU의 일정 용량?을 차지하면서 수행하는 주체가 Process입니다.**

하여, 프로그램은 하나인데 Process는 여러 개가 동작할 수 있습니다. (==인스턴스가 여러 개)

예를 들어 엑셀을 사용하고 있는데, 새로 만들기로 엑셀창을 하나 더 열 수도 있겠죠? 여기서 엑셀 실행파일이 Program, 엑셀창이 Process입니다. <b>간단히 1개의 Program으로 2개의 Process를 동작시킨 것입니다.</b>

​	

🙋‍♂️ 이제 프로세스(Process)의 특징에 대하여 좀 더 알아봅시다.

1. 프로세스는 각각 독립된 메모리 영역(Code, Data, Stack, Heap의 구조)을 할당받습니다.

2. 기본적으로 프로세스당 최소 1개의 스레드(메인 스레드)를 가지고 있습니다.

3. 각 프로세스는 별도의 주소 공간에서 실행되며, <b>한 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없습니다</b>.

4. <b>BUT</b> 한 프로세스가 다른 프로세스의 자원에 접근하려면 프로세스 간의 통신(IPC, inter-process communication)을 사용하면 가능해집니다. Ex) 파이프, 파일, 소켓통신, mapped file 등을 이용한 통신 방법 이용

   _ex) 실제로 엑셀의 경우 A엑셀파일, B엑셀파일이 있을 때, B의 특정 셀값을 A에서 불러와 사용할 수 있습니다. 즉, 프로세스간에 통신이 가능합니다._

   ​		

   ​	

# <span style="color: #dabc9a">Thread</span>

`프로세스(Process) 내에서 실행되는 흐름의 단위`으로 
프로세스 하나에 자원을 공유하면서 일련의 과정, 여러 개를 동시에 실행시킬 수 있는 것을 말합니다.

기본적으로 하나의 프로세스가 생성되면 하나의 Thread가 같이 생성됩니다. 이를 `Main Thread`라고 부르며, thread를 추가로 생성하지 않는 한, 모든 프로그램 코드는 Main Thread에서 실행됩니다. 

{{<image src="../images/ProcessVSThread_01.png" caption="single Thread의 경우" width="550px" style="display: block; margin-left: auto; margin-right: auto; margin-top: 20px">}}

​		

{{<image src="../images/ProcessVSThread_03.png" caption="Multi Thread의 경우" width="550px" style="display: block; margin-left: auto; margin-right: auto; margin-top: 20px">}}

Multi Thread 그림을 보면 

- 한 프로세서 내의 주소 공간이나 자원들을 대부분 공유하고 _( Stack만 스레드 별로 가지게 됩니다. )_

- 하나의 프로세스는 여러 개의 스레드를 가질 수 있는데, 이를 `Multi Thread`라고 합니다.

- 한 스레드가 프로세스 자원을 변경하면, 다른 이웃 스레드(sibling thread)도 그 변경 결과를 즉시 볼 수 있습니다.

  ​		

🙋‍♂️ 다른 자원들은 공유하지만 굳이 스택만 분리해서 사용하는 이유는? 

> LIFO(Last In First Out) / 후입 선출이라는 스택의 특성과도 연관이 있습니다.
> 왜냐하면?  Code와 Data, Heap 영역을 공유하는 것에는 큰 문제가 없지만,
> 스택 영역은 스택이 쌓이면 위에서부터 프로세스가 섞인 채로 순서대로 나오게 되므로
> 더 복잡해지기 때문에 원활한 실행 흐름을 위해 스택은 따로 독립적으로 존재하게 됩니다.

​		

​	

# Multi <span style="color: #358691">Process</span>       

>하나의 응용프로그램을 여러 개의 프로세스로 구성하여 각 프로세스가 하나의 작업(태스크)을 처리하도록 하는 것.

{{<image src="../images/ProcessVSThread_04.png" caption="하나의 운영프로그램을 여러개의 Process가 처리" width="550px" style="display: block; margin-left: auto; margin-right: auto; margin-top: 20px">}}

😆 장점

- 안정성이 좋음 - <span style="color: grey; font-size: .9rem">여러 개의 자식 프로세스 중 하나에 문제가 발생하면 그 자식 프로세스만 죽는 것 이상으로 다른 영향이 확산되지 않습니다.</span>

😔 단점

- Context Switching<span style="color: grey; font-size: .7rem">(여러 프로세스를 왔다 갔다 하는 것)</span>에서의 **오버헤드**

  - Context Switching 과정에서 캐쉬 메모리 초기화 등 무거운 작업이 진행되고 많은 시간이 소모되는 등의 오버헤드가 발생하게 됩니다.
  - 프로세스는 각각의 독립된 메모리 영역을 할당받았기 때문에 프로세스 사이에서 공유하는 메모리가 없어, Context Switching가 발생하면 캐쉬에 있는 모든 데이터를 모두 리셋하고 다시 캐쉬 정보를 불러와야 합니다.

- 프로세스 사이의 어렵고 복잡한 통신 기법(IPC)

  ​	

<p style='text-align:right; font-style:italic; color:grey; font-size:.9rem'>*오버헤드(overhead)는 어떤 처리를 하기 위해 들어가는 간접적인 처리 시간 · 메모리 등을 말한다.</p>

​	

​	

​	

**이런 단점들을 보완하기 위해 Multi <span style="color: #dabc9a">Thread</span>가 등장하였습니다.**

# Multi <span style="color: #dabc9a">Thread</span>

>하나의 프로세스 내에서 여러 개의 thread가 동작하는 것을 말합니다.

{{<image src="../images/ProcessVSThread_05.png" caption="하나의 운영프로그램을 여러개의 Process가 처리" width="550px" style="display: block; margin-left: auto; margin-right: auto; margin-top: 20px">}}

😆 장점

- 시스템 효율성이 향상됩니다. 
  - 프로세스에서 자원을 할당하는 콜이 줄어들어 자원을 효율적으로 관리할 수 있습니다.
  - 즉, 시스템의 자원 소모를 줄일 수 있습니다.
- 시스템 처리량이 증가 됩니다. 
  - 스레드 사이에 작업량이 작아 Context Switching이 빠릅니다.
- 결과적으로 프로그램 응답 시간이 단축됩니다.
- 프로세스 간 통신 방법에 비해 thread간의 통신 방법이 간단합니다.<br> _✔ 스레드는 Stack 영역을 제외한 모든 메모리를 공유하기 때문_

- 프로세스 간의 전환 속도보다 스레드 간의 전환 속도가 빠르다.
  <br>_✔ Context Switching시 스레드는 Stack 영역만 처리하기 때문_

😔 단점

- 여러 개의 스레드를 이용하는 경우, 미묘한 시간차나 잘못된 변수를 공유함으로써 오류가 발생할 가능성이 있습니다.  하여 **Thread-Safety**하게 구현해야 합니다. <span style="color: grey; font-size: .9rem">ex) 스레드1이 공유 자원 내의 A데이터를 조작하다가, 스레드2에 제어권을 넘겨준 이후 스레드2가 A데이터를 변경한다면 스레드1이 다시 제어권을 받아 남은 작업을 계속할 때 원치 않는 결과가 나올 수 있다.</span>

- 프로그램 디버깅이 어렵습니다.

- 단일 프로세스 시스템에서는 효과를 기대하기 어렵습니다. 

  ​		

  ​	

## Thread-Safety

>스레드 안전(thread 安全, 영어: thread safety)은 멀티 스레드 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다. 보다 엄밀하게는 하나의 함수가 한 스레드로부터 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하여 동시에 함께 실행되더라도 각 스레드에서의 함수의 수행 결과가 올바로 나오는 것으로 정의한다.

1. **Re-entrancy(재진입성)**

   - 어떤 함수가 한 스레드에 의해 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하더라도 그 결과가 각각에게 올바로 주어져야 한다. <span style="color: grey; font-size: .8rem">(동기화 잘 하라.)</span>

2. **Mutual exclusion(상호 배제)**

   - [공유 자원](https://ko.wikipedia.org/wiki/공유_자원)을 꼭 사용해야 할 경우 해당 자원의 접근을 [세마포어](https://ko.wikipedia.org/wiki/세마포어) 등의 [락](https://ko.wikipedia.org/w/index.php?title=락_(컴퓨터과학)&action=edit&redlink=1)으로 통제한다. <span style="color: grey; font-size: .8rem">(역시 동기화 잘 하라.ㅎ.)</span>

3. **Thread-local storage(스레드 지역 저장소)**

   - 공유 자원의 사용을 최대한 줄여 각각의 스레드에서만 접근 가능한 저장소들을 사용함으로써 동시 접근을 막는다. <span style="color: grey; font-size: .8rem">(글로벌 변수 등의 사용을 남발하지 말라.)</span>

4. **Atomic operations(원자 연산)**

   - 공유 자원에 접근할 때 원자 연산을 이용하거나 '원자적'으로 정의된 접근 방법을 사용함으로써 상호 배제를 구현할 수 있다. <span style="color: grey; font-size: .8rem">(역시 동기화 잘 하라는 건데, python 연산자를 예로 "+="는 한줄의 코드 임에도 "+"를 한 후에 "=" 연산을 하기 때문에 원자적이라고 할 수 없다고 봅니다.)</span>

     ​	

---

### 부록

- 자바에서 동기화 방법
  1. Synchronized
  2. Volatile
  3. Atomic Class

---

​	

​	

# 👀요약

> 운영체제는 Process 단위로 **메모리를 할당**해주고, 같은 프로세스 소속의 **Thread는 메모리를 공유**합니다. <span style="color: grey; font-size: .9rem">(스레드도 각자의 스택영역을 보유하므로 완전히 공유한다고 보기는 어렵습니다.)</span>

​	

​	













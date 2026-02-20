# langchain & langgraph



## LangChain & LangGraph

> 대화형 주문 인공지능을 개발하면서, 사용했던 프레임워크인 LangChain & Langgraph에 대하여 정리하고 사용방법을 정리한다.

​	

## LangChain

LangChain은 대형 언어 모델(LLM)을 중심으로 한 애플리케이션을 쉽게 구성할 수 있도록 하는 프레임워크이다. 

풀어서 말하면

- LLM을 **API처럼 쓰는 것에 그치지 않고**
- 프롬프트, 도구, 메모리, 외부 데이터(RAG) 등을
- **하나의 애플리케이션 구조로 묶어주는 추상화 계층**

​	

​	

## LangGraph

LangGraph는 상태(State)를 기반으로 LLM 애플리케이션의 <b>실행 흐름을 그래프 구조로 정의</b>하기 위한 프레임워크이다. (여기서 중요한 키워드는 "실행 흐름을 그래프 구조로 정의")

풀어서 말하면

- LLM 호출을 단순히 “한 번 실행”하는 게 아니라
- **상태를 가진 프로세스**로 보고

- 각 단계를 **노드(Node)**, 흐름을 **엣지(Edge)** 로 정의

​	

​	

## 실제 개발 순서

> 실제 개발하는 데 진행했던 순서를 정리한다. 

1. 사용할 모델을 정의한다.
2. state를 정의한다.
3. 대략적인 그래프 구조를 고안한 후 필요한 node를 정리한다.
   - 단일 기능으로 동작?할 노드
   - should_continue로 분기할 노드
4. 그래프를 정의하고 컴파일한다.
5. checkpointer 설정한다.
6. api와 연동하여 .invoke()하여 사용한다.

보통 이런 순서로 진행하면 크게 문제 없이 동작할 것이며, 큰 흐름은 이해할 수 있다. (다만 이 흐름과 상관 없이 데이터를 저장하고 관리해야 하는데 이는 redis를 사용하는 등 분산 처리 및 관리하는 것을 권장한다.)

​	

​	

#### 1. 사용할 모델을 정의한다. 

> 필자는 azure에 있는 모델을 사용했으며 이를 불러오는 방법 등은 아래 코드를 참고하면 되겠다.

```python
# models.py

from langchain_openai import AzureChatOpenAI

from config import settings

llm = AzureChatOpenAI(
    azure_deployment="gpt-4o",
    api_version="2024-08-01-preview",
    api_key=settings.AZURE_OPENAI_API_KEY,
    azure_endpoint=settings.AZURE_OPENAI_ENDPOINT,
)
```

여기서 중요한 값은 "azure_deployment", "api_version"가 홈페이지에 등록된 내용과 정확히 일치해야 한다.

​	

​	

#### 2. state를 정의한다. 

> 사실상 지금 단계에서 이를 완벽히 수행하기는 어렵다고 생각한다. 다만 어떤 값들을 주고 받을 것이며, 어떤 데이터 들을 다룰 것인지 미리 정의 한다고 생각. (경험상 반드시 구조가 강제되지는 않는 부분이다.)

```python
# schemas.py

from typing import Any, Dict, List
from langgraph.graph import MessagesState


class AgentSystemSchema(MessagesState):
    """
    에이전트 전반에서 공통으로 참조하는 시스템 상태 정의
    (비즈니스 컨텍스트 및 시스템 고정 정보)
    """

    system_message: str
    # LLM에게 전달되는 시스템 프롬프트

    products: List[Dict[str, Any]]
    # 주문 가능한 상품 목록 (상품명, 가격, 옵션 등 비즈니스 정보)

    previous_orders: str
    # 사용자의 이전 주문 내역 요약 정보
    # 대화 맥락 유지 및 재주문/추천 판단에 활용


class AgentState(MessagesState):
    """
    단일 대화 흐름에서 변경되며 누적되는 에이전트 실행 상태
    """

    message: str
    # 가장 최근 사용자 또는 에이전트의 발화

    messages: List[Any]
    # 전체 대화 이력
    # 메시지 누적 관리 및 LLM 컨텍스트 제공 용도

    summary: str
    # messages가 과도하게 누적되는 것을 방지하기 위한
    # 기존 대화 요약 정보 (컨텍스트 압축 목적)

    system: AgentSystemSchema
    # 시스템 전역 상태
    # 대화 중 변경되지 않거나 드물게 변경되는 비즈니스/환경 정보

```

​	

​	

#### 3. 대략적인 그래프 구조를 고안한 후 필요한 node를 정리한다.

> node는 역할 기준으로 다음 두 가지로 구성하였다.
>
> 1. **작업 수행 노드**
>    - 단일 책임을 가지는 로직을 수행하며
>    - state를 갱신하고 다음 단계로 전달할 결과를 반환한다.
> 2. **분기 판단 노드 (should_continue)**
>    - 현재 state를 기준으로
>    - 그래프의 실행 흐름을 제어한다.

​	

작업 수행 노드 예시

```python
## 사용자의 발화가 주문인지 아닌지 판단

from langchain_core.output_parsers import JsonOutputParser
from langchain_core.prompts import PromptTemplate

from ..llm.models import llm
from ..schemas import AgentState

order_detection_prompt = PromptTemplate.from_template("""
당신은 사용자의 입력이 주문인지(혹은 주문의 수정, 삭제인지) 아닌지 판별하는 주문 판별 전문 AI 어시스턴트입니다.
무조건 아래 schema를 준수해서 JSON 형태로 답을 해야 합니다.

class OrderState(str, Enum):
    right_order = "right_order"
    wrong_order = "wrong_order"
    not_order = "not_order"

class OrderDetectionSchema(BaseModel):
    is_order: OrderState
    system_message: str

사용자 입력:
{messages}

구매할 수 있는 제품목록:
{available_products}

이전 주문 내역:
{previous_orders}


주문 여부를 판단 기준
1. 제품명이 존재하는지?
- '사용자 입력'에는 반드시 상품명과 수량이 포함되어 있어야 함
- 사용자는 공식 상품명을 그대로 쓰지 않고, 약어나 고유 키워드만으로 표현하는 경우가 많으므로 이 점을 반드시 고려할 것
- '유사한 상품명'에 대한 정보와 반드시 비교해서 유사한 상품이 있는지 알아본 후 주문인지 아닌지 고려할 것.
- '제품 목록'에 100% 일치하는 상품이 있는 경우, 이 상품을 주문하는 것으로 판단할 것
- 사용자가 입력한 상품명은 제공된 '구매할 수 있는 제품목록'과 반드시 연관성이 있어야 하며, 부분적으로 일치하거나 키워드 유사도(예: "만두"가 "고기 만두" 또는 "김치 만두"와 부분적으로 일치하는 경우)가 있다면, 정확히 일치하지 않아도 유효한 상품명으로 인식하세요. 유사한 제품이 복수일 때에도 주문으로 간주할 것
- 주문 여부 판단 시, 상품명이 매우 추상적일 수 있음을 염두에 두고, 연관성이 다소 낮더라도 제품 정보에 유사한 단어나 키워드가 있으면 '주문'을 한거라고 판단할 것

2. 수량이 존재하는지?
- 수량은 상품명에 직접 붙어 있을 수도 있고, 별도의 단위와 함께 숫자로 표기될 수도 있습니다. 단, 수량은 항상 정수로 표현됩니다.
- 수량이 각각의 상품명 끝에 직접 붙어 있을 수 있으며, 이 경우 각 숫자를 해당 상품의 주문 수량으로 해석해야 합니다(예: "갓김치1"은 제품 '갓김치 1개 주문', "참이슬1"은 제품 '참이슬 1개 주문').
- 상품명 뒤에 공백 없이 숫자가 바로 따라오면(예: "배추김치103"), 그 숫자를 해당 상품의 주문 수량으로 해석해야 합니다. 즉, "배추김치103"은 상품명 "배추김치"에 수량 103개로 인식해야 합니다.
- 수량은 1, 10, 100, 1000, 10000 이상도 가능하며, 이는 특이한 케이스가 아니고 수량을 의미한다고 판단할 것
- 이전 주문 내역(previous_orders)을 참고해서 주문 여부를 판단할 것. 이전 주문 내역과 유사한 형태로 입력했다면 이는 주문일 확률이 높습니다.
- 주문할 수 없는 상품에 대한 주문으로 판단되면, 해당되는 상품 전체를 언급하며 다시 정확히 주문해달라고 system_message 필드에 작성해 주세요.
(ex. 말씀하신 {messages}는 주문으로 판단되지 않습니다.)

주의사항
위 기준을 바탕으로 입력을 꼼꼼히 분석한 뒤, 주문이면 is_order 필드에 주어진 Enum에 맞는 값으로 반환해주세요.
is_order에는 <class 'str'>타입의 'right_order', 'wrong_order', 'not_order' 이외에는 어떤 값도 들어갈 수 없습니다. 또 다른 어떤 문자나 특수문자를 추가하지 마세요. 꼭 <class 'str'>로 값이 들어와야 합니다.
system_message에는 <class 'str'> 타입으로 반환해야 합니다.

당부사항
❗system_message에는 사용자의 입력을 왜 주문으로 판단했는지, 어떤 상품 정보를 비교했는지, 어떤 상품에 유사하다고 판단했는지, 상품명과 수량이 잘 표시되었는지, 결과적으로 왜 주문으로 판단했는지 그 근거를 반드시 한글로 작성해 주세요.
""")


async def is_order_should_continue(state: AgentState):
    messages = state["messages"]
    available_products = (
        state["system"].get("buyer_info_for_order", {}).get("products_info", [])
    )
    ...

    rag_chain = order_detection_prompt | llm | JsonOutputParser()
    
    answer = await rag_chain.ainvoke(
        {
            "messages": messages,
            "available_products": available_products,
            "previous_orders": previous_orders,
            "similar_products": similar_products,
        }
    )

    if answer["is_order"] == "right_order":
        return "order_parser"
    elif answer["is_order"] == "wrong_order":
        return "wrong_order_parser"
    elif answer["is_order"] == "not_order":
        return "invite_branch"
    else:
        return "safe_exit"

```

여기서 라인별로 중요한? 부분이 있는데 

```python
rag_chain = order_detection_prompt | llm | JsonOutputParser()
```

이 부분은 LangChain의 **Runnable 조합 모델**을 사용하는 핵심 로직이다.  Prompt, LLM, OutputParser를 각각 독립적인 Runnable로 정의한 뒤,  이를 파이프라인 형태로 합성하여 하나의 실행 단위(chain)로 구성한다.

먼저 Prompt와 OutputParser를 통해 **LLM의 출력 형식을 명확히 정의**한 뒤,  그 결과를 해석하여 **다음에 실행할 노드를 return 값으로 결정한다. ** LangGraph는 이 return 값을 기준으로 그래프 상의 다음 노드로 상태를 전달한다.



다음, should_continue 노드 예시를 보자.

```python
# is_invite_should_continue.py

from langchain_core.prompts import PromptTemplate
from langgraph.graph import END

from ..llm.models import llm
from ..schemas import AgentState
from .utils.boolean_output_parser import BooleanOutputParser

invite_detection_prompt = PromptTemplate.from_template("""
사용자 입력:
{messages}

- 입력된 문자열은 반드시 6자리여야 하며, 오직 숫자 0부터 9까지의 숫자로만 이루어져 있어야 합니다.  
- 띄어쓰기, 한글, 영어, 특수문자, 기타 어떤 문자도 포함되어 있으면 안 됩니다.

반환 값 조건:
- 출력은 반드시  <class 'bool'> 타입의 True 또는 False 만 반환하세요.

예시:
- '495038' → True  
- '000123' → True  
- '67384' → False (5자리)  
- '1234567' → False (7자리 이상)  
- '123 456' → False (띄어쓰기 포함)  
- 'ABC123' → False (영어 포함)  
- '123!@#' → False (특수문자 포함)
""")


async def is_invite_should_continue(state: AgentState):
    messages = state["messages"][-1].content

    rag_chain = invite_detection_prompt | llm | BooleanOutputParser()
    response = await rag_chain.ainvoke({"messages": messages})

    if response:
        return "invite_parser"
    return END
```

비교적 간단한다. 상위 단계에서 사용자의 발화가 주문인지 아닌지 판단한 후 만약 주문이 아니라면 초대코르를 입력한 것인지 판단하기 위한 코드이다. 

​	

여기까지 봤다면 어느정도 감이 올 것인데, 간단히 <b>LangChain은 “무엇을 판단할지”, LangGraph는 “어디로 흐를지”를 담당한다.</b>

사실 이 단계와 그래프를 빌드 하는건 거의 동시에 수행된다. 왜냐하면 노드 자체에 이런 흐름을 정의해야 하고, 이 정의에 맞게 그래프를 빌드하기 때문이다. 

​	

​	

#### 4. 그래프를 정의하고 컴파일한다.

```python
# graph.builder.py

from langgraph.checkpoint.memory import MemorySaver
from langgraph.graph import END, START, StateGraph

from ..nodes import (
    init_order,
    invite_branch,
    invite_parser,
    is_invite_should_continue,
    is_order_should_continue,
    order_confirm_should_continue,
    order_parser,
    ...
)
from ..schemas import AgentState

## graph_builder 설정 - node 추가
graph_builder = StateGraph(AgentState)

graph_builder.add_node("invite_branch", invite_branch)
graph_builder.add_node("invite_parser", invite_parser)
graph_builder.add_node("init_order", init_order)
...

## graph_builder 설정 - edge 추가
graph_builder.add_conditional_edges(
    START, order_confirm_should_continue, ["update_messages", "init_order"]
)
graph_builder.add_edge("update_messages", "retrieve")
graph_builder.add_edge("retrieve", "similar_products")
graph_builder.add_conditional_edges(
    "similar_products",
    is_order_should_continue,
    ["order_parser", "wrong_order_parser", "invite_branch", "safe_exit"],
)
...
```

대략 이런 식으로 구성한다. 

​	

​	

#### 5. checkpointer 설정한다.

checkpointer는 LangGraph 실행 중의 state를 저장하고 복구하기 위한 상태 저장 장치다.

조금 풀면,

- 그래프 실행 도중
- 각 노드가 끝날 때마다
- **현재 state를 외부 저장소에 기록**해 두는 역할

만약 이 설정이 없다면, 인공지능은 "대화"를 기억하고 답변하는 게 아니라 당장의 사용자 발화만을 인식하게 될 것인다. 

​	

해당 설정은 매우 간단하다.

```python
from langgraph.checkpoint.memory import MemorySaver

from .graph.builder import graph_builder

# langgraph 체크포인터 설정(히스토리 모드를 위해 사용)
checkpointer = MemorySaver()

# langgraph 그래프 컴파일
graph = graph_builder.compile(checkpointer=checkpointer)
```

이렇게 그래프를 컴파일할 때 인자로 넣어주면 된다. 

​	

​	

#### 6. api와 연동하여 .ainvoke()하여 사용한다.

사용은 더 간단하다. 

```python
import asyncio
from typing import Any

import httpx
from fastapi import APIRouter, Request
from langchain_core.messages import HumanMessage
from langgraph.checkpoint.memory import MemorySaver

from .graph.builder import graph_builder

order_ai_router = APIRouter()

# langgraph 체크포인터 설정(히스토리 모드를 위해 사용)
checkpointer = MemorySaver()

# langgraph 그래프 컴파일
graph = graph_builder.compile(checkpointer=checkpointer)

async def get_ai_response(message: str, plusfriend_user_key: str):
    config = {"configurable": {"thread_id": user_pk}}

    result = await graph.ainvoke(
        {
            "message": [HumanMessage(message)],
            "plusfriend_user_key": plusfriend_user_key,
        },
        config=config,
    ) 
    return result   
```

이런 식으로 컴파일된 값들을 가지고와서 .ainvoke() 혹은 .invoke()을 해서 추론한 값을 그대로 return 해주면 된다.

**`.invoke()`** → 동기 실행 (blocking)

**`.ainvoke()`** → 비동기 실행 (`async/await`)

​	

​	

​	

​	

## 마무리

해당 구조를 활용해 RAG나 프롬프트 엔지니어링 등 다양한 기법을 적용할 수 있으니 큰 흐름과 구조만 잘 알아두자. 

​	

​	

​	

​	

​	

​	



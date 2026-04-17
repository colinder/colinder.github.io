# what is Sqlachemy

<br>

# what is sqlachemy?
python에서 관계형 데이터베이스를 효율적으로 다루기 위한 강력한 SQL 툴킷 및 ORM(Object-Relational Mapping) 라이브러리

<br>
SQLAlchemy Core와 ORM은 파이썬 객체를 데이터베이스의 테이블과 컬럼처럼 사용할 수 있게 하기 위해서 만들어졌습니다. 이러한 것들을 데이터베이스 메타데이터로 사용할 수 있습니다.

> 메타데이터는 데이터를 기술하는 데이터를 설명합니다. 여기서 메타데이터는 구성된 테이블, 열, 제약 조건 및 기타 객체 정보 등을 말합니다.
> 

<br><br>
SQLAlchemy 는 ORM 방식으로 저장하는데 우선

메타 데이터(`metadata`)를 생성하고 이를 활용하는 것이 기본 개념이자 로직이다. 

**기본 클래스(Base) 정의:**

- `declarative_base`를 호출하면 기본 클래스를 생성합니다. 이 클래스는 모든 ORM 모델 클래스의 기반(base) 클래스로 사용됩니다.
- 이 기본 클래스는 `SQLAlchemy`의 ORM 시스템에 필요한 메타데이터를 저장하며, 이 메타데이터는 테이블 정의와 매핑 정보가 포함됩니다.

**매핑 정보 저장:**

- `Base` 클래스를 통해 정의된 모델 클래스들은 테이블의 메타데이터와 매핑 정보를 `Base.metadata`에 저장합니다.
- 이 메타데이터는 나중에 데이터베이스에 실제 테이블을 생성하거나 업데이트하는 데 사용됩니다.

<br>

```python
# _base.py
from sqlalchemy import Column, MetaData

from sqlalchemy.orm import declarative_base
from sqlalchemy.sql import func

metadata = MetaData()

_Base = declarative_base(metadata=metadata)

class Base(_Base):  # type: ignore
    __abstract__ = True

    def __repr__(self) -> str:
        columns = ", ".join(
            [
                f"{k}={repr(v)}"
                for k, v in self.__dict__.items() if not k.startswith("_")
            ]
        )
        return f"<{self.__class__.__name__}({columns})>"

```

```python
from sqlalchemy import (
    Column, 
    Integer, 
    String, 
)

from ._base import Base

class Alias(Base):
    __tablename__ = "alias"

    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=True)
```

- **테이블 생성 및 매핑:**
    - 모델 클래스를 정의한 후, `Base.metadata.create_all(engine)`을 호출하여 실제 데이터베이스에 테이블을 생성할 수 있습니다.
    - 이 단계에서는 메타데이터를 기반으로 데이터베이스에 테이블이 생성됩니다.
    
    ### 주의!! `Base.metadata.create_all(engine)`의 사용을 권장하지 않습니다.
    
    alembic을 사용할 것이기 때문에 
    
    > sqlahcemy로는 ORM을 이용한 DB 스키마 구성만 하고 alembic으로 실제 DB에 반영하는 작업을 합니다. 수정하는 경우 sqlahcemy로는 ORM로 수정하고 alembic으로 반영하고 로직으로 진행하는 것을 권장
    > 
    <br>

- **세션 관리:**
    - `Session` 객체를 생성하여 데이터베이스와 상호작용합니다. 세션은 데이터베이스에 대한 트랜잭션을 관리하고, 모델 인스턴스를 데이터베이스에 삽입하거나 조회하는 데 사용됩니다.
    
<br>
또 엔진이라는 개념이 등장하는데 
<br><br><br>

### SQLAlchemy 엔진

**1. 엔진의 역할:**

- SQLAlchemy의 **엔진**은 데이터베이스와의 연결을 관리하는 핵심 구성 요소입니다. 엔진은 데이터베이스에 직접 연결하여 SQL 쿼리를 실행하고 결과를 반환합니다.
- 엔진은 데이터베이스와의 통신을 위한 낮은 수준의 인터페이스를 제공하며, SQLAlchemy ORM을 사용할 때도 기본적으로 이 엔진을 통해 데이터베이스와 상호작용하게 됩니다.

**2. 엔진 생성:**

```python
from sqlalchemy import create_engine

engine = create_engine(DATABASE_URL, echo=True)
```

- `create_engine` 함수는 데이터베이스 URL을 사용하여 엔진 객체를 생성합니다. `DATABASE_URL`은 데이터베이스에 대한 모든 연결 정보를 포함하는 문자열입니다.
- `echo=True`는 SQLAlchemy가 실행하는 모든 SQL 쿼리를 콘솔에 출력하여 디버깅에 도움이 됩니다.

**3. 엔진의 구성 요소:**

- **Connection Pool**: 엔진은 데이터베이스와의 연결을 관리하기 위한 커넥션 풀을 사용합니다. 커넥션 풀은 여러 데이터베이스 연결을 미리 생성하여 필요할 때 재사용함으로써 성능을 향상시킵니다.
- **Dialect**: SQLAlchemy는 다양한 데이터베이스 시스템을 지원하는 다양한 '방언'(dialect)을 사용합니다. 방언은 데이터베이스 시스템의 특정 SQL 문법 및 기능을 이해하고 처리합니다.
- **Engine Metadata**: 엔진은 메타데이터를 포함하고 있으며, 이는 데이터베이스의 구조에 대한 정보를 저장하고, SQL 쿼리를 실행하는 데 필요한 정보를 제공합니다.
<br><br><br>

### SQLAlchemy 메타데이터

**1. 메타데이터의 역할:**

- **메타데이터**는 데이터베이스의 구조를 설명하는 데이터입니다. SQLAlchemy에서 메타데이터는 테이블, 열, 관계 등의 정보를 포함합니다.
- 메타데이터는 ORM 모델 클래스가 데이터베이스 테이블에 어떻게 매핑되는지를 정의하는 데 사용됩니다.

**2. 메타데이터와 Base 클래스:**

```python
from sqlalchemy.orm import declarative_base

Base = declarative_base()
```

- `declarative_base` 함수는 ORM 모델 클래스가 상속받을 기본 클래스를 생성합니다. 이 기본 클래스는 메타데이터를 저장하고, 모든 모델 클래스는 이 클래스를 상속받아 데이터베이스 테이블과의 매핑을 정의합니다.
- 모델 클래스에서 정의한 테이블 및 열 정보는 `Base.metadata`를 통해 메타데이터에 저장됩니다.

**3. 메타데이터 사용:**

- **테이블 생성**: 메타데이터를 사용하여 실제 데이터베이스에 테이블을 생성할 수 있습니다.
    
    ```python
    Base.metadata.create_all(engine)
    ```
    
    - `create_all` 메서드는 메타데이터에 정의된 모든 테이블을 데이터베이스에 생성합니다.
<br><br><br>

### SQLAlchemy 세션

**1. 세션의 역할:**

- **세션**은 데이터베이스 트랜잭션을 관리하고, ORM 모델과 데이터베이스 간의 상호작용을 수행하는 객체입니다. 세션은 데이터베이스 작업을 추적하고, 커밋 또는 롤백을 통해 트랜잭션을 관리합니다.
- 세션은 객체와 데이터베이스 사이의 상호작용을 추상화하여, 객체 지향적으로 데이터베이스 작업을 수행할 수 있게 합니다.

**2. 세션 생성:**

```python
from sqlalchemy.orm import sessionmaker

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
```

- `sessionmaker`는 세션을 생성하는 팩토리 함수를 반환합니다. 이 팩토리 함수는 세션 객체를 만들기 위한 설정을 제공합니다.
- `autocommit=False`는 세션이 자동으로 커밋되지 않도록 설정합니다. 수동으로 `session.commit()`을 호출해야 데이터베이스에 변경 사항이 반영됩니다.
- `autoflush=False`는 세션이 자동으로 데이터를 플러시하지 않도록 설정합니다. 명시적으로 `session.flush()`를 호출해야 데이터베이스에 변경 사항이 반영됩니다.
- `bind=engine`은 세션이 데이터베이스와 연결된 엔진을 사용하도록 설정합니다.

**3. 세션 사용:**

- **세션 객체 생성**: `SessionLocal`을 호출하여 세션 객체를 생성합니다.
    
    ```python
    session = SessionLocal()
    ```
    
- **데이터베이스 작업**: 세션 객체를 사용하여 데이터베이스에 데이터를 추가하거나 조회할 수 있습니다.
    
    ```python
    # 객체 생성 및 추가
    new_user = User(name='John Doe', email='john@example.com')
    session.add(new_user)
    session.commit()
    
    # 객체 조회
    user = session.query(User).filter_by(name='John Doe').first()
    ```
<br><br><br>

### 요약

- **엔진**은 데이터베이스와의 연결을 관리하고 SQL 명령을 실행하는 기본 구성 요소입니다. 데이터베이스와의 모든 상호작용은 엔진을 통해 이루어집니다.
- **메타데이터**는 데이터베이스의 구조에 대한 정보를 저장하고, ORM 모델이 데이터베이스 테이블과 어떻게 매핑되는지를 정의합니다. `Base.metadata`를 통해 테이블을 생성하거나 업데이트할 수 있습니다.
- **세션**은 데이터베이스 트랜잭션을 관리하고, ORM 모델과 데이터베이스 간의 상호작용을 수행합니다. 세션은 데이터베이스 작업을 추적하고 커밋 또는 롤백을 통해 트랜잭션을 제어합니다.

이러한 구성 요소들은 SQLAlchemy를 통해 데이터베이스와의 효과적인 상호작용을 가능하게 합니다.
<br><br><br>

### 트랜잭션의 예시

**은행 송금 예시**

- 트랜잭션 시작: 계좌 A에서 계좌 B로 송금하는 작업을 시작합니다.
- 작업 1: 계좌 A에서 송금할 금액을 차감합니다.
- 작업 2: 계좌 B에 송금된 금액을 추가합니다.
- 트랜잭션 완료: 두 작업이 모두 성공하면 트랜잭션을 커밋하여 변경 사항을 확정합니다.
- 트랜잭션 실패: 만약 중간에 오류가 발생하면 두 작업 모두 롤백하여 송금 작업을 취소합니다.
<br><br><br>

### SQLAlchemy에서의 트랜잭션 관리

SQLAlchemy에서는 세션을 사용하여 트랜잭션을 관리합니다. 세션은 트랜잭션을 시작하고, 변경 사항을 추적하며, 트랜잭션이 성공적으로 완료되면 커밋하고, 실패하면 롤백합니다.

**트랜잭션 시작과 커밋**

```python
session = SessionLocal()  # 세션 생성
try:
    # 데이터베이스 작업 수행
    user = User(name='John Doe', email='john@example.com')
    session.add(user)

    # 커밋하여 변경 사항을 확정
    session.commit()
except Exception as e:
    # 예외 발생 시 롤백
    session.rollback()
    raise e
finally:
    # 세션 종료
    session.close()
```

- **세션 생성**: 세션을 생성하면 새로운 트랜잭션이 시작됩니다.
- **작업 수행**: 세션을 통해 데이터베이스 작업을 수행합니다.
- **커밋**: `session.commit()`을 호출하면 트랜잭션이 커밋되어 변경 사항이 데이터베이스에 반영됩니다.
- **롤백**: 오류가 발생하면 `session.rollback()`을 호출하여 트랜잭션을 롤백하고 변경 사항을 취소합니다.
- **세션 종료**: 트랜잭션이 완료되면 `session.close()`로 세션을 종료합니다.

트랜잭션을 통해 데이터베이스의 신뢰성과 무결성을 보장할 수 있으며, 세션을 사용하여 이를 효과적으로 관리할 수 있습니다.


# what is sqlachemy

<br>

# sqlachemy / alembic
sqlachemy와 alembic의 관계와 기본 설정에 관한 고찰

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
<br>

**매핑 정보 저장:**

- `Base` 클래스를 통해 정의된 모델 클래스들은 테이블의 메타데이터와 매핑 정보를 `Base.metadata`에 저장합니다.
- 이 메타데이터는 나중에 데이터베이스에 실제 테이블을 생성하거나 업데이트하는 데 사용됩니다.

<br><br>

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

# Alembic

> Python 으로 `SQLAlchemy` 를 사용하고 있을 때 데이터베이스를 관리해주는 마이그레이션 도구
> 

설명을 이해해보자 `SQLAlchemy` 로 기반을 만들고 `Alembic` 으로 변경사항을 실제 DB에 반영한다. 는 로직으로 개발하면 된다.

### 0. 설치

```bash
pip install alembic
```

### 1. 초기화

alembic을 사용하기 위해서는 dependency 추가가 선행되어야 한다.

```
$> alembic init 폴더명
```

위 명령어를 수행하면 작성한 폴더명으로 alembic 정보가 담길 폴더가 생성된다.

폴더명을 'alembic'으로 입력하면 **'alembic'이라는 이름의 폴더**와 **'alembic.ini'라는 파일**이 생성된다.

**그런데 이중에는 데이터베이스 연결을 위한 설정을 해야 하는데 이는 .env 관리하는 것이 좋을 것같아서 조금 복잡하게 설명하겠다.** 

### 2. 환경 설정

보통의 경우 사용자가 관리해주어야 하는 파일은 총 2개

1. alembic 폴더 안에 **env.py**
    - 스키마의 변경사항을 감지할 수 있게 Base.metadata를 지정해주는 파일
2. **alembic.ini**
    - 데이터 베이스 연결을 위한 설정을 하는 파일

초기 설정시 권장하는 구조는 아래과 같다.

- **env.py (에서 총 2가지)**
    1. `SQLAlchemy` 에 생성한 Base의 metadata를 target_metadata로 등록해준다. 
    2. DB 연결할 url 설정 (?이건 alembic.ini 에서 한다고 했는데?. 일단 보자.)
        - db 연결 주소를 .env로 관리하는데 이를 config.py에서 가져올 수 있게 하자.
            - 조금 길다… 펼쳐보자.
                - 일단 아래와 같이 구조를 만들었고,
                
                - .env
                    
                    ```bash
                    # .env.py
                    MYSQL_HOSTNAME=192.168.30.23
                    MYSQL_PORT=3306
                    MYSQL_DB_NAME=mydatabase
                    MYSQL_USERNAME=imuser
                    MYSQL_PASSWORD=imuser_password
                    
                    ```
                    
                - 우선 DB 연결을 위한 주소를 가져올 config.py 파일을 생성했다.
                    
                    ```bash
                    from os import path
                    from pydantic import ValidationError
                    from pydantic_settings import BaseSettings, SettingsConfigDict
                    from pydantic.networks import MySQLDsn, MultiHostUrl
                    from dotenv import load_dotenv
                    
                    load_dotenv()
                    
                    API_PREFIX = "/api"
                    
                    class ProjectConfig(BaseSettings):
                        model_config = SettingsConfigDict(case_sensitive=False)
                        debug: bool = False
                    
                        project_name: str = "cnia-backend"
                    
                    class DatabaseConfig(BaseSettings):
                        model_config = SettingsConfigDict(case_sensitive=False)
                    
                        mysql_hostname: str
                        mysql_port: int
                        mysql_db_name: str
                        mysql_username: str
                        mysql_password: str
                    
                        @property
                        def database_uri(self) -> str:
                            dsn: MySQLDsn = MultiHostUrl.build(
                                scheme="mysql+pymysql",
                                host=self.mysql_hostname,
                                port=self.mysql_port,
                                username=self.mysql_username,
                                password=self.mysql_password,
                                path=f'{self.mysql_db_name}'
                            )
                            return dsn.unicode_string()
                    
                    try:
                        project_config = ProjectConfig()
                        db_config = DatabaseConfig()    
                    except ValidationError as ex:
                        error("REQUIRED ENV SETTING (DB_HOSTNAME, DB_PORT, DB_USERNAME, DB_PASSWORD, DB_NAME)")
                        raise ex
                    ```
                    
        - 이제 config.py에 정리한 내용을 가져와서 **env.py에 적용하자**
            
            ```bash
            from logging.config import fileConfig
            
            from sqlalchemy import engine_from_config, pool
            
            from alembic import context
            
            from db.orm import Base  # SQLAlchemy에서 만든 스키마 구조 base가져오고
            from config import db_config  # db 접속 정보 가져와서
            
            # this is the Alembic Config object, which provides
            # access to the values within the .ini file in use.
            config = context.config
            
            # Interpret the config file for Python logging.
            # This line sets up loggers basically.
            if config.config_file_name is not None:
                fileConfig(config.config_file_name)
            
            # add your model's MetaData object here
            # for 'autogenerate' support
            # from myapp import mymodel
            # target_metadata = mymodel.Base.metadata
            ### target_metadata = None
            target_metadata = Base.metadata  # SQLAlchemy base의 메타데이터 등록하고
            
            # other values from the config, defined by the needs of env.py,
            # can be acquired:
            # my_important_option = config.get_main_option("my_important_option")
            # ... etc.
            
            ## 함수를 만들어서 접속 정보 가져오자.
            ## 여기서 mysql+pymysql 이 부분은 기본적으로 알렘빅은 동기식 관리 라이브러리이기 때문에 
            ## 동기식 구성으로 적어야 한다. (dialect+driver 구문) 즉 여기는 알렘빅과 동기식으로 통신할 구조를 명시
            def get_sqlalchemy_url() -> str:
                return (
                    f"mysql+pymysql://{db_config.mysql_username}:{db_config.mysql_password}@"
                    f"{db_config.mysql_hostname}:{db_config.mysql_port}/{db_config.mysql_db_name}"
                )
            
            def run_migrations_offline() -> None:
                # url = config.get_main_option("sqlalchemy.url")  # 디폴트 값 주석처리
                url = get_sqlalchemy_url()  # 이렇게 활용하고
                context.configure(
                    url=url,
                    target_metadata=target_metadata,
                    literal_binds=True,
                    dialect_opts={"paramstyle": "named"},
                )
            
                with context.begin_transaction():
                    context.run_migrations()
            
            def run_migrations_online() -> None:
            		# 아래와 같이 등록해주자. sqlalchemy.url 값을 지정해주는 코드이다.
                configuration = config.get_section(config.config_ini_section)
                if "sqlalchemy.url" not in configuration:
                    configuration["sqlalchemy.url"] = get_sqlalchemy_url()
                
                connectable = engine_from_config(
                    configuration,  # 여기 주의해서 변경해주자
                    prefix="sqlalchemy.",
                    poolclass=pool.NullPool,
                )
                
                with connectable.connect() as connection:
                    context.configure(
                        connection=connection, target_metadata=target_metadata
                    )
            
                    with context.begin_transaction():
                        context.run_migrations()
            
            if context.is_offline_mode():
                run_migrations_offline()
            else:
                run_migrations_online()
            
            ```
            
    
- **alembic.ini**
    
    > 데이터베이스의 접속 정보를 관리하는 파일
    > 
    
    보통의 경우 
    
    ```bash
    sqlalchemy.url = driver://user:pass@localhost/dbname
    ```
    
    이 값이 DB를 연결하는 주소를 등록하는 부분이다. 그런데 위에서 sqlalchemy.url 값을 모두 지정할 수 있게 설정하였음으로, 그냥 주석처리 해주면 된다. 
    

지금까지 설정한 사항들을 실제로 DB에 반영해보자. 

### 3. 설정 사항 반영

alembic은 **revision**이라는 것으로 버전을 관리하고 이를 반영한다.  

revision은 변경사항을 저장할 건데 index를 지정하는 것과 비슷하다.

> 내가 새롭게 변경한 사항의 리비전을 만들겠다. > 새롭게 변경한 사항을 묶어서 하나의 index로 설정하겠다. 는 의미.
> 

```
# 새로운 revision 추가
$ alembic revision                # (권장X)
$ alembic revision -m "message"   # 메세지를 추가하여 등록 (권장X)

# 읭? 둘다 권장하지 않는다고?
```

해당 명령어를 입력하면 version 폴더 안에 새로운 파일이 생기는데, 리비전의 변경사항이 기록된 파일이 생긴 것이다. 

```python
# Script 예시

"""{commit 메세지}

Revision ID: e19af14ddb13
Revises:
Create Date: 2023-01-10 22:10:21.507160

"""
from alembic import op
import sqlalchemy as sa

# revision identifiers, used by Alembic.
revision = 'e19af14ddb13'
down_revision = None
branch_labels = None
depends_on = None

def upgrade():
    pass

def downgrade():
    pass
```

보통 이런 형태인데 다음 명령어를 입력한 뒤 차이를 보면서 이해하자.

```bash
# alembic에서 변화를 자동적으로 감지해 revision을 작성해줌
$ alembic revision --autogenerate                # (권장X) 
$ alembic revision --autogenerate -m "message"   # 메세지를 추가하여 등록 (권장)
```

- autogenerate 옵션을 사용하지 않고 revision을 진행하면 직접 변화를 작성해주어야한다.
    
    문장이 이해하기 어려울 수 있으니 아까 version 폴더 안에 새로생긴 파일을 보자.
    

```bash
"""first migrations

Revision ID: b14910929b0e
Revises: 9a54d99dec89
Create Date: 2024-08-26 14:11:31.573926

"""
from typing import Sequence, Union

from alembic import op
import sqlalchemy as sa

# revision identifiers, used by Alembic.
revision: str = 'b14910929b0e'
down_revision: Union[str, None] = '9a54d99dec89'
branch_labels: Union[str, Sequence[str], None] = None
depends_on: Union[str, Sequence[str], None] = None

def upgrade() -> None:
    # ### commands auto generated by Alembic - please adjust! ###
    op.create_table('alias',
    sa.Column('id', sa.Integer(), autoincrement=True, nullable=False),
    sa.Column('name', sa.String(length=50), nullable=True),
    sa.PrimaryKeyConstraint('id')
    )
    op.create_table('battlefield',
    sa.Column('id', sa.Integer(), autoincrement=True, nullable=False),
    sa.Column('drone_id', sa.String(length=50), nullable=False),
    sa.Column('alias_id', sa.Integer(), nullable=False),
    sa.Column('threat', sa.Float(), nullable=True),
    sa.PrimaryKeyConstraint('id')
    )
    op.create_table('battlefield_locations',
    sa.Column('id', sa.Integer(), autoincrement=True, nullable=False),
    sa.Column('battlefield_id', sa.Integer(), nullable=False),
    sa.Column('latitude', sa.String(length=50), nullable=True),
    sa.Column('longitude', sa.String(length=50), nullable=True),
    sa.Column('altitude', sa.Float(), nullable=True),
    sa.Column('created_at', sa.DateTime(), nullable=False),
    sa.PrimaryKeyConstraint('id')
    )
    
    # ### end Alembic commands ###

def downgrade() -> None:
    # ### commands auto generated by Alembic - please adjust! ###
    op.drop_table('battlefield_locations')
    op.drop_table('battlefield')
    op.drop_table('alias')
    # ### end Alembic commands ###

```

이 기록도 중간에 살짝 꼬인 것이라 이해가 조금 어려울 수 있으나, 큰변화를 이해하면 된다. 

아까. [autogenerate 옵션을 사용하지 않고 revision을 진행하면 직접 변화를 작성해주어야한다.](https://www.notion.so/c6fde6b7d75f4582aceaab973a7cd470?pvs=21)고 했는데 autogenerate를 사용하지 않으면 변경된 부분을 수기로 작성해야 한다. (난 하기 싫다.)

하여 이렇게 명령어로 작성하고 난 후 간단히 변경사항을 검토하자 문법적으로 크게 어렵지는 않다. 

이제 이 모든 변경사항을 실제 DB에 반영하자

```bash
alembic upgrade head
```

DB를 보면 사항들이 반영되어 있어야 한다. 만약 에러가 발생한다면, 

천천히 오류를 잡아보자. 

## 이후 ORM 구성의 CRUD 작업시

1. orm 변경하고
2. alembic revision --autogenerate -m "message"  로 커밋하고
3. alembic upgrade head 로 최신화 하면 된다. 

[SQLalchemy](https://www.notion.so/SQLalchemy-1d18df5faccb80cf9620c15ac8aeb7cf?pvs=21)

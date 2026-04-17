# what is Alembic

<br>

# what is alembic?
> `SQLAlchemy` 를 사용하고 있을 때 데이터베이스를 관리해주는 마이그레이션 도구
> 

설명을 이해해보자 `SQLAlchemy` 로 기반을 만들고 `Alembic` 으로 변경사항을 실제 DB에 반영한다. 는 로직으로 개발하면 된다.
<br><br>

0. 설치
    ```bash
    pip install alembic
    ```
<br>

1. 초기화
    alembic을 사용하기 위해서는 dependency 추가가 선행되어야 한다.

    ```
    $ alembic init 폴더명
    ```

    위 명령어를 수행하면 작성한 폴더명으로 alembic 정보가 담길 폴더가 생성된다.

    폴더명을 'alembic'으로 입력하면 **'alembic'이라는 이름의 폴더**와 **'alembic.ini'라는 파일**이 생성된다.

    **그런데 이중에는 데이터베이스 연결을 위한 설정을 해야 하는데 이는 .env 관리하는 것이 좋을 것같아서 조금 복잡하게 설명하겠다.** 
<br><br>

2. 환경 설정

    보통의 경우 사용자가 관리해주어야 하는 파일은 총 2개
    <br>
    1. alembic 폴더 안에 **env.py**
        - 스키마의 변경사항을 감지할 수 있게 Base.metadata를 지정해주는 파일
    2. **alembic.ini**
        - 데이터 베이스 연결을 위한 설정을 하는 파일
    
    <br>
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
        
    <br>
    지금까지 설정한 사항들을 실제로 DB에 반영해보자. 

<br>

3. 설정 사항 반영

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
    <br>
    
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
    <br>

    보통 이런 형태인데 다음 명령어를 입력한 뒤 차이를 보면서 이해하자.

    ```bash
    # alembic에서 변화를 자동적으로 감지해 revision을 작성해줌
    $ alembic revision --autogenerate                # (권장X) 
    $ alembic revision --autogenerate -m "message"   # 메세지를 추가하여 등록 (권장)
    ```

    - autogenerate 옵션을 사용하지 않고 revision을 진행하면 직접 변화를 작성해주어야한다.
        
        문장이 이해하기 어려울 수 있으니 아까 version 폴더 안에 새로 생긴 파일을 보자.
        

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
    <br>

    하여 이렇게 명령어로 작성하고 난 후 간단히 변경사항을 검토하자 문법적으로 크게 어렵지는 않다. 
    <br><br>

    이제 이 모든 변경사항을 실제 DB에 반영하자

    ```bash
    alembic upgrade head
    ```

    DB를 보면 사항들이 반영되어 있어야 한다. 만약 에러가 발생한다면, 

    천천히 오류를 잡아보자. 
<br><br><br>

## 이후 ORM 구성의 CRUD 작업시

1. orm 변경하고
2. alembic revision --autogenerate -m "message"  로 커밋하고
3. alembic upgrade head 로 최신화 하면 된다. 
4. 간단하다.

<br><br><br><br>

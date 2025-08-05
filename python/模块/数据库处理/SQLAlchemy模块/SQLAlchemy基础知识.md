# 一、初识SQLAlchemy

 SQLAlchemy2.0的特点

- 使用Python编写的
- 一个SQL工具包
- 一个ORM框架

支持常见的数据库:Microsoft SQLServer,MySQL,Oracle, PostgresQL, SQLite, Sybase

## 安装

```
pip install sqlalchemy
```

## 查看版本

```python
# 查看版本 3条命令
python
import sqlalchemy
sqlalchemy.__version__
```

![image-20250723112105237](imge/SQLAlchemy基础知识.assets/image-20250723112105237.png)

## 连接数据库

### 第一步：创建数据库引擎

#### 创建sqlite数据库引擎

```python
from sqlalchemy import create_engine

# 创建引擎 echo=True 会打印SQL语句,方便调试,生产环境建议关闭
# SQLite连接方式，默认使用sqlite3引擎，无需安装第三方库，直接使用
# db文件不存在会自动创建
engine = create_engine('sqlite:///test.db', echo=True)
```

#### 创建MySQL数据库引擎

```python
from sqlalchemy import create_engine

# 创建引擎 echo=True 会打印SQL语句,方便调试,生产环境建议关闭
# MYSQL数据库默认MySQLdb引擎，如需使用其它引擎，需要通过【mysql+引擎】指明，推荐pymysql
engine = create_engine('mysql+pymysql://root:qazwsx@localhost:3306/塔租费用', echo=True)
```

### 第二步：连接数据库

#### 直连方式

- 低级SQL核心接口，接近原生SQL
- 需要手动处理结果集映射
- 直接执行SQL语句
- connect 实现面向表操作

```python
"""省略创建引擎过程"""

# 创建连接
connection = engine.connect()

# 关闭连接
connection.close()
# 关闭引擎
engine.dispose()
```

#### ORM方式

- 高级ORM接口，面向对象操作
- 自动管理对象状态（如脏数据检测）
- session 实现面向对象操作
- 创建session有2种方式：

  - Session(engine)

  - sessionmaker(bind=engine) 推荐这种

    通过sessionmake方法创建一个Session工厂，然后在调用工厂的方法来实例化一个Session对象


**方法一：创建事务，半自动 commit**

提交数据 不需要commit提交事务

```python
from sqlalchemy.orm import Session
from contextlib import contextmanager

"""省略创建引擎过程"""

# @contextmanager 是 Python 标准库 contextlib 模块提供的一个装饰器，
# 用于将一个生成器函数转换为上下文管理器。它的主要作用是简化上下文管理器的创建过程。
@contextmanager
def db_getter():
    """
    获取数据库连接
    """
    # 创建会话2种方式
    ## 第一种
    with Session(engine) as session:
        # 创建一个新的事务，半自动 commit
        with session.begin():
            # 事务提交
            yield session
```

**方法二：创建事务**

提交数据 需要commit提交事务

```python
from sqlalchemy.orm import Session
from contextlib import contextmanager

"""省略创建引擎过程"""

# @contextmanager 是 Python 标准库 contextlib 模块提供的一个装饰器，
# 用于将一个生成器函数转换为上下文管理器。它的主要作用是简化上下文管理器的创建过程。
@contextmanager
def db_getter():
    """
    获取数据库连接
    """
    # 创建会话2种方式
    ## 第一种
    with Session(engine) as session:
            yield session
```

## 查询数据库

使用直连方式，操作原始SQL语句

```python
from sqlalchemy import text

query = text("SELECT * FROM students")
result = connection.execute(query)  # result是sqlalchemy.engine.cursor.CursorResult对象
print(result.all())  # 返回[(),(),()]
```

# 二、创建表

## sql语句

```python

sql = '''create table student(
    id int not null primary key,
    name varchar(50),
    age int,
    address varchar(100));
'''

"""省略创建引擎和连接数据库过程"""

conn.execute(sql)
```



## core方式

- MetaData类主要用于保存表结构，连接字符串等数据，是一个多表共享的对象
- meta_data = MetaData(engine)    #绑定一个数据源的metadata
- meta_data.create_all(engine)         #是来创建表，这个操作是安全的操作，会先判断表是否存在。

```python
"""
程序说明：
    功能：创建表
    使用MetaData,Table,Column以及字段类型在代码中来创建表
"""
# here put the import lib
from sqlalchemy import  MetaData, Table, Column, Integer, String

"""省略创建引擎过程"""
 
meta_data = MetaData()

students = Table(
    'students',
    meta_data,
    Column('id', Integer, primary_key=True),
    Column('name', String(50), ),
    Column('age', Integer),
    Column('address', String(10)),
)

meta.create_all(engine)
```

Table 参数说明：

```tex
name    表名
 metadata      共享的元数据
 *args： Column 是列定义
 **kwargs 定义：
 schema 此表的结构名称，默认None
 autoload 自动从现有表中读入表结构，默认False
 autoload_with 从其他engine读取结构，默认None

include_columns 如果autoload设置为True，则此项数组中的列明将被引用，没有写的列明将被忽略，None表示所有都列明都引用，默认None
 mustexist 如果为True，表示这个表必须在其他的python应用中定义，必须是metadata的一部分，默认False
 useexisting 如果为True，表示这个表必须被其他应用定义过，将忽略结构定义，默认False
 owner 表所有者，用于Orcal，默认None
 quote 设置为True，如果表明是SQL关键字，将强制转义，默认False
 quote_schema  设置为True，如果列明是SQL关键字，将强制转义，默认False
 mysql_engine  mysql专用，可以设置'InnoDB'或'MyISAM'
```

Column参数说明：

```text
1、name 列名
 2、type_ 类型，更多类型 sqlalchemy.types
 3、*args： Constraint（约束）,  ForeignKey（外键）,  ColumnDefault（默认）, Sequenceobjects（序列）定义
 4、key 列名的别名，默认None
 **kwargs：
 5、primary_key 如果为True，则是主键
 6、nullable 是否可为Null，默认是True
 7、default 默认值，默认是None
 8、index 是否是索引，默认是True
 9、unique 是否唯一键，默认是False
 10、onupdate 指定一个更新时候的值，这个操作是定义在SQLAlchemy中，不是在数据库里的，当更新一条数据时设置，大部分用于updateTime这类字段
 11、autoincrement 设置为整型自动增长，只有没有默认值，并且是Integer类型，默认是True
 12、quote 如果列明是关键字，则强制转义，默认False
```











```python
"""
程序说明：
    功能：创建表
    共有2种建表方式
    1、声明式(Declarative) - ORM方式（推荐）
    2、命令式(Imperative) - Core方式
    3、命令式 可以继承Base 也可以继承 metadata
"""
# here put the import lib
from sqlalchemy import (
    String,
    ForeignKey,
    create_engine,
    MetaData,
    Table,
    Column,
    DateTime,
    Integer,
    func,
    Boolean,
    inspect,
)
from sqlalchemy.orm import Mapped, mapped_column, declared_attr
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import DeclarativeBase
from datetime import datetime

# 创建引擎（连接数据库）
engine = create_engine("sqlite:///mydatabase.db", echo=True)


class Base(DeclarativeBase):
    """
    创建基本映射类
    稍后，我们将继承该类，创建每个 ORM 模型
    """

    @declared_attr.directive
    @classmethod
    def __tablename__(cls) -> str:
        """
        表名检测

        如果有自定义表名就取自定义，没有就取小写类名
        """
        table_name = cls.__tablename__
        if not table_name:
            model_name = cls.__name__
            ls = []
            for index, char in enumerate(model_name):
                if char.isupper() and index != 0:
                    ls.append("_")
                ls.append(char)
            table_name = "".join(ls).lower()
        return table_name


# 方法1：声明式(Declarative) - ORM方式（推荐）
# 使用类定义表结构，更适合ORM操作
class BaseModel(Base):
    """
    公共 ORM 模型，基表
    """

    __abstract__ = True

    id: Mapped[int] = mapped_column(Integer, primary_key=True, comment='主键ID')
    create_datetime: Mapped[datetime] = mapped_column(
        DateTime,
        server_default=func.CURRENT_TIMESTAMP(),
        comment='创建时间',  # noqa: E1102
    )
    update_datetime: Mapped[datetime] = mapped_column(
        DateTime,
        server_default=func.CURRENT_TIMESTAMP(),
        onupdate=func.CURRENT_TIMESTAMP(),
        comment='更新时间',
    )
    delete_datetime: Mapped[datetime | None] = mapped_column(
        DateTime, nullable=True, comment='删除时间'
    )
    is_delete: Mapped[bool] = mapped_column(
        Boolean, default=False, comment="是否软删除"
    )


class VadminDept(BaseModel):
    __tablename__ = "vadmin_auth_dept"  # type: ignore
    __table_args__ = {'comment': '部门表'}

    name: Mapped[str] = mapped_column(
        String(50), index=True, nullable=False, comment="部门名称"
    )
    dept_key: Mapped[str] = mapped_column(
        String(50), index=True, nullable=False, comment="部门标识"
    )
    disabled: Mapped[bool] = mapped_column(Boolean, default=False, comment="是否禁用")
    order: Mapped[int | None] = mapped_column(Integer, comment="显示排序")
    desc: Mapped[str | None] = mapped_column(String(255), comment="描述")
    owner: Mapped[str | None] = mapped_column(String(255), comment="负责人")
    phone: Mapped[str | None] = mapped_column(String(255), comment="联系电话")
    email: Mapped[str | None] = mapped_column(String(255), comment="邮箱")

    parent_id: Mapped[int | None] = mapped_column(
        Integer,
        ForeignKey("vadmin_auth_dept.id", ondelete='CASCADE'),
        comment="上级部门",
    )


# 方法2：命令式(Imperative) - Core方式
# 直接使用Table对象定义表结构，更接近原生SQL
vadmin_auth_user_roles = Table(
    "vadmin_auth_user_roles",
    # 注意区别
    Base.metadata,
    Column("user_id", Integer, comment="用户id"),
    Column("role_id", Integer, comment="角色id"),
)

# 创建所有表（两种方法选其一）
# 方法1：使用声明式的Base,配合第一种建表方法
# 继承 Base 的表都会被创建
Base.metadata.create_all(engine)

# # 方法2：使用命令式的metadata
# metadata = MetaData(engine)

# vadmin_auth_user_roles = Table(
#     "vadmin_auth_user_roles",
#     # 注意区别
#     metadata,
#     Column("user_id", Integer, comment="用户id"),
#     Column("role_id", Integer, comment="角色id"),
# )

# metadata.create_all(engine)

```







# 三、增删改查

## 添加数据

### sql语句

省略

### core方式

- **没有返回值**

#### 添加一条数据

```python
from init.create_table import engine, vadmin_auth_user_roles

# 生成sql语句 
# INSERT INTO vadmin_auth_user_roles (user_id, role_id) VALUES (:user_id, :role_id)
insert_sql = vadmin_auth_user_roles.insert()
insert_data =insert_sql.values(user_id='1', role_id='1')
with engine.connect() as conn:
    # 执行
    result = conn.execute(insert_data)
    # 提交
    conn.commit()
```

#### 批量插入

```python
from init.create_table import engine, vadmin_auth_user_roles
# 生成sql语句 
# INSERT INTO vadmin_auth_user_roles (user_id, role_id) VALUES (:user_id, :role_id)
insert_sql = vadmin_auth_user_roles.insert()
with engine.connect() as conn:
    # 执行
    result = conn.execute(
        insert_sql,
        [
            {'user_id': 2, 'role_id': 2},
            {'user_id': 3, 'role_id': 3},
            {'user_id': 4, 'role_id': 4},
        ],
    )
    # 提交
    conn.commit()
```

## 查询数据

### sql语句

- 返回[(),(),()]

#### 查询所有数据

```python
from sqlalchemy import text

"""省略创建引擎过程"""

query = text("SELECT * FROM `vadmin_auth_user_roles`;")
with engine.connect() as conn:
    result = conn.execute(query)  # result是sqlalchemy.engine.cursor.CursorResult对象
    print(result.all())  # 返回[(),(),()]
```

#### 条件查询

```python
from sqlalchemy import text

"""省略创建引擎过程"""

# 条件查询
query = text("SELECT * FROM `vadmin_auth_user_roles` WHERE user_id > 3;")
with engine.connect() as conn:
    result = conn.execute(query)  # result是sqlalchemy.engine.cursor.CursorResult对象
    print(result.all())  # 返回[(),(),()]
```



### core方式

- 返回[(),(),()]

#### 查询所有数据

```python
from init.create_table import vadmin_auth_user_roles

"""省略创建引擎过程"""

# 生成sql语句
select_sql = vadmin_auth_user_roles.select()
with engine.connect() as conn:
    result = conn.execute(select_sql)
    print(result.all())  # [(),()]
```

#### 条件查询

```python
from init.create_table import vadmin_auth_user_roles

"""省略创建引擎过程"""

## 单条件
# select_sql = vadmin_auth_user_roles.select().where(vadmin_auth_user_roles.c.user_id > 3)
## 多条件 且的关系
select_sql = (
    vadmin_auth_user_roles.select()
    .where(vadmin_auth_user_roles.c.user_id > 1)
    .where(vadmin_auth_user_roles.c.role_id > 2)
)
## 复杂查询 and_、or_、in_
select_sql = vadmin_auth_user_roles.select().where(
    and_(vadmin_auth_user_roles.c.user_id > 1, vadmin_auth_user_roles.c.role_id > 2)
)
with engine.connect() as conn:
    result = conn.execute(
        select_sql
    )  # result是sqlalchemy.engine.cursor.CursorResult对象
    print(result.all())  # [(),()]
```

## 修改数据

### sql语句

```python
```



### core方式



## 删除数据

## ORM方式

使用的是ORM方式连接数据库，会自动提交数据，此处省略session.commit()

### 添加数据

```python
"""
程序说明：
    功能：添加数据
    共有3种添加方式
    1、声明式(Declarative) - ORM方式（推荐）
    2、命令式(Imperative) - Core方式
    3、批量分块添加
"""
from sqlalchemy import insert
from create_table import engine, vadmin_auth_user_roles, VadminDept
from get_db import db_getter

with db_getter() as session:
    # 声明式(Declarative) - ORM方式 插入数据 （对象实例化）

    # 情况1：需要获取插入后的对象（如需要自增ID）
    # ================================批量数据插入=============================
    # 方法1
    departments = [
        VadminDept(name="总经办", dept_key="ceo", order=0),
        VadminDept(name="研发中心", dept_key="rd", order=1),
        VadminDept(name="产品部", dept_key="product", order=2),
    ]
    # add_all 会逐个检测对象状态，不适合大规模插入,会返回主键
    departments_result = session.add_all(departments)
    # 方法2
    departments_result2 = session.execute(
        insert(VadminDept),
        [
            {"name": "总经办", "dept_key": "ceo", "order": 0},
            {"name": "研发中心", "dept_key": "rd", "order": 1},
            {"name": "产品部", "dept_key": "product", "order": 2},
        ],
    )
    # 获取插入后的对象主键
    print("departments2:", departments_result2)

    # ===================================单个数据添加=============================
    # 方法1
    dept2 = VadminDept(
        name="前端组", dept_key="frontend", parent_id=1, order=1  # 关联父部门ID
    )
    dept2_result = session.add(dept2)
    # 方法2
    departments2 = insert(VadminDept).values(name="产品部22", dept_key="product")
    departments2_result = session.execute(departments2)
    # 获取插入后的对象主键
    departments2 = departments2_result.inserted_primary_key
    print("departments2:", departments2)

    # 情况2：批量插入子部门（不需要立即获取对象）
    # 使用 bulk_insert_mappings 批量插入部门，性能最优，不会返回主键
    session.bulk_insert_mappings(
        VadminDept,
        [
            {"name": "总经办", "dept_key": "ceo", "order": 0},
            {"name": "研发中心", "dept_key": "rd", "order": 1},
            {"name": "产品部", "dept_key": "product", "order": 2},
            {"name": "前端组", "dept_key": "frontend", "parent_id": 1, "order": 1},
        ],
    )

    # 超大数据量插入（10万+），建议：
    # 分批次插入（每1000条提交一次）
    # pip install more-itertools
    # from more_itertools import chunked

    # for chunk in chunks(big_data, 1000):
    #     session.bulk_insert_mappings(VadminDept, chunk)

    # 命令式(Imperative) - Core方式 插入数据 （直接执行SQL构造）,性能最快，推荐
    # =================================单个数据======================================
    role_assignment = vadmin_auth_user_roles.insert().values(user_id=1, role_id=1)
    # 执行插入
    role_assignment_result = session.execute(role_assignment)
    print("role_assignment_result:", role_assignment_result)
    # ===================================批量数据====================================
    role_assignment_results = session.execute(
        vadmin_auth_user_roles.insert(),
        [{"user_id": 3, "role_id": 3}, {"user_id": 2, "role_id": 2}],
    )
    print("role_assignment_results:", role_assignment_results)
```

### 查询数据

```python
from sqlalchemy import select, and_
from create_table import VadminDept, vadmin_auth_user_roles
from get_db import db_getter

with db_getter() as session:
    # 声明式(Declarative) - ORM方式 查询数据
    # 查询所有
    # 方法1 传统 ORM 查询方式 返回List[ORM对象]
    departments = session.query(VadminDept).all()
    print(departments)

    # 方法2 SQLAlchemy 2.0+ 推荐方式 返回ScalarResult[ORM对象]（可迭代）
    # 混合使用（ORM + 核心）
    sql = select(VadminDept)  # 生产原生 SQL
    departments2 = session.scalars(sql)
    print(departments2.all())

    # 方法3 核心 SQL 风格 返回数据列表 最优（接近原生 SQL）
    query = vadmin_auth_user_roles.select()
    result = session.execute(query)
    print(result.all())

    # ======================================条件查询===========================
    # 方法1 传统 ORM 查询方式
    departments = session.query(VadminDept).filter(VadminDept.id == 1).all()
    print(departments)

    # 方法2 SQLAlchemy 2.0+ 推荐方式
    # and_ 为 SQLAlchemy 2.0+ 新增的函数，用于构建 and 条件
    # or_ 为 SQLAlchemy 2.0+ 新增的函数，用于构建 or 条件
    sql = select(VadminDept).where(
        and_(
            *[
                VadminDept.name == "总经办",
                VadminDept.dept_key == "ceo",
            ]
        )
    )
    departments2 = session.scalars(sql)
    print(departments2.all())

    # 方法3 核心 SQL 风格
    query = select(vadmin_auth_user_roles).where(
        and_(
            *[
                vadmin_auth_user_roles.c.user_id == 1,
                vadmin_auth_user_roles.c.role_id == 1,
            ]
        )
    )
    result = session.execute(query)
    print(result.all())
```

### 筛选数据

where、filter、filter_by

**性能对比**

| 方法              | 适用场景        | 性能特点                                                  | 推荐使用场景                      |
| ----------------- | --------------- | --------------------------------------------------------- | --------------------------------- |
| **`where()`**     | SQLAlchemy Core | 直接生成原生 SQL，性能最优（适合高频、复杂查询）          | 需要极致性能或直接操作 SQL 时使用 |
| **`filter()`**    | ORM             | 转换为 SQL 时有一定开销，但灵活性高（适合复杂条件）       | 大多数 ORM 查询场景               |
| **`filter_by()`** | ORM（简化版）   | 语法糖，最终转换为 `filter()`，性能与 `filter()` 几乎相同 | 简单**等值**查询                  |

**使用示例**

```python
import time
from sqlalchemy import select

# ORM filter 
    session.query(User).filter(User.name == "Alice").all()
    # 多条件组合
	session.query(User).filter(User.name == "Alice",User.age > 30).all()
	# 使用逻辑运算符
	session.query(User).filter(or_(User.name == "Alice", User.name == "Bob")).all()


# ORM filter_by 简单等值查询
    session.query(User).filter_by(name="Alice").all()
    # 多字段等值查询
	session.query(User).filter_by(name="Alice", age=30).first()

# Core where
	stmt = select(User).where(User.name == "Alice")
  	# 多条件查询 下面2种方式 效果一样  
    v_where = [VadminDept.name == "总经办",VadminDept.dept_key == "ceo" ]
    sql1 = select(VadminDept).where(and_(*v_where))
    sql2 = select(VadminDept).where(
        and_(
            VadminDept.name == "总经办",
            VadminDept.dept_key == "ceo",
        )
    )

```

### 查询方式

query、select，具体返回数据类型需结合查询方法

| 特性         | `query` (ORM)          | `select` (Core)               |
| ------------ | ---------------------- | ----------------------------- |
| **接口类型** | 面向对象               | 面向 SQL                      |
| **返回结果** | ORM 对象（如模型实例） | 原始数据（如元组）            |
| **性能**     | 稍慢（需 ORM 转换）    | 更快（直接生成 SQL）          |
| **适用场景** | 常规业务逻辑           | 高频查询、复杂 SQL 或批量操作 |

 **混合使用示例**

ORM 和 Core 可以互相转换：

```python
from sqlalchemy.orm import aliased

# ORM 转 Core
user_alias = aliased(User)
stmt = select(user_alias).where(user_alias.age > 30)
result = session.execute(stmt).scalars().all()  # 返回 ORM 对象
```

### 查询方法

| 特性             | `execute()`                   | `scalars()`               |
| ---------------- | ----------------------------- | ------------------------- |
| **返回结果**     | 原始数据（如元组或 `Result`） | 第一列的标量值或 ORM 对象 |
| **适用查询类型** | Core 和 ORM                   | Core 和 ORM               |
| **典型用途**     | 需要多列数据或原生结果时      | 只需要第一列或 ORM 对象时 |
| **性能**         | 稍低（需处理多列）            | 更高（只处理一列）        |

**使用示例**

```python
from sqlalchemy import select

# query 可直接查询，无需查询方法
# query + ORM
# 返回[ORM对象,ORM对象,...]
departments = session.query(VadminDept).all()

# query + Core 无此方法

# query + ORM + execute
# 返回 [(ORM对象,),...]
query_orm_sql = session.query(VadminDept)
query_orm = session.execute(query_orm_sql).all()

# query + ORM + scalars
# 返回 [ORM对象,ORM对象,...]
query_orm_sql = session.query(VadminDept)
query_orm = session.scalars(query_orm_sql).all()


# select 不能单独使用，必须配合查询方法
# select + ORM + execute
# 返回 [(ORM对象,),...]
query_orm_sql = select(VadminDept)
query_orm = session.execute(query_orm_sql).all()

# select + ORM + scalars  SQLAlchemy 2.0+ 推荐方式
# 返回 [ORM对象,ORM对象,...]
query_orm_sql = select(VadminDept)
query_orm = session.scalars(query_orm_sql).all()

# select + Core + execute
# 返回 [(行数据),...]
query_orm_sql = vadmin_auth_user_roles.select()
query_orm = session.execute(query_orm_sql).all()

# select + Core + scalars
# [第一行第一列,第二行第一列,...]
query_orm_sql = vadmin_auth_user_roles.select()
query_orm = session.scalars(query_orm_sql).all()
```

### 更新数据

```python
from sqlalchemy import select, and_, update
from create_table import VadminDept, vadmin_auth_user_roles
from get_db import db_getter

with db_getter() as session:
    # ========================方法1 Core 方式（直接操作表） ==============================
    # 更新数量和查询数量有关（单个或批量）
    stmt = (
        update(vadmin_auth_user_roles)
        .where(vadmin_auth_user_roles.c.user_id == 1)
        .values(role_id=2)
    )
    session.execute(stmt)

    # ========================方法2 OMR方式（需定义模型类） ==============================
    # 只能更新单个对象
    dept = session.query(VadminDept).filter(VadminDept.name == "批量更新部门").first()
    if dept:
        # 方法1
        dept.name = "更新后的总经办2"
        dept.dept_key = "updated_ceo"
        # 方法2：
        for key, value in {
            "name": "更新后的总经办2",
            "dept_key": "updated_ceo",
        }.items():
            setattr(dept, key, value)

    # 更新数量和查询数量有关（单个或批量）
    dept = session.query(VadminDept).filter(VadminDept.id > 5)
    if dept:
        dept.update({"name": "批量更新部门", "email": "2025-07-23"})

    # ===========================方法3 混合方式（Core + ORM）==================================
    # 更新数量和查询数量有关（单个或批量）
    stmt = (
        update(VadminDept)
        .where(VadminDept.id > 5)
        .values(name="批量更新部门", email="2025-07-23")
    )
    session.execute(stmt)
```



### 删除数据

```python
from sqlalchemy import select, and_, delete
from init.create_table import VadminDept, vadmin_auth_user_roles
from init.get_db import db_getter

with db_getter() as session:
    # ========================方法1 Core 方式（直接操作表） ==============================
    # 删除数量和查询数量有关（单个或批量）
    stmt = delete(vadmin_auth_user_roles).where(vadmin_auth_user_roles.c.user_id == 1)
    session.execute(stmt)

    # ========================方法2 OMR方式（需定义模型类） ==============================
    # 删除数量和查询数量有关（单个或批量）
    dept = session.query(VadminDept).filter(VadminDept.name == "批量更新部门").delete()

    # ===========================方法3 混合方式（Core + ORM）==================================
    # 更新数量和查询数量有关（单个或批量）
    stmt = delete(VadminDept.__table__).where(VadminDept.id > 5)
    session.execute(stmt)
```

# 四、关联表之一对一

## 表定义

通过在子表创建外键约束实现关联

```python
"""
程序说明：
    功能：创建表
    共有2种建表方式
    1、声明式(Declarative) - ORM方式（推荐）
    2、命令式(Imperative) - Core方式
    此处使用ORM方式创建表
    重点注意：外键约束，关系映射
"""
# here put the import lib
from datetime import datetime
from sqlalchemy import (
    String,
    ForeignKey,
    create_engine,
    Table,
    Column,
    DateTime,
    Integer,
    func,
    Boolean,
)
from sqlalchemy.orm import (
    DeclarativeBase,
    Mapped,
    mapped_column,
    declared_attr,
    relationship,
)
from .get_db import engine


class Base(DeclarativeBase):
    """
    创建基本映射类
    稍后，我们将继承该类，创建每个 ORM 模型
    """

    pass  #  代码省略


# 方法1：声明式(Declarative) - ORM方式（推荐）
# 使用类定义表结构，更适合ORM操作
class BaseModel(Base):
    """
    公共 ORM 模型，基表
    """

    pass  # 代码省略


class User(BaseModel):
    __tablename__ = "users"  # type: ignore
    __table_args__ = {'comment': '用户表'}

    username: Mapped[str] = mapped_column(  # pylint:disable=unsubscriptable-object
        String(50), nullable=False, comment="用户名"
    )
    email: Mapped[str | None] = mapped_column(
        String(100), nullable=True, comment="邮箱"
    )

    # 定义一对一关系
    profile: Mapped["Profile"] = relationship("Profile", back_populates="user")


class Profile(BaseModel):
    __tablename__ = "profiles"  # type: ignore
    __table_args__ = {'comment': '用户资料表'}

    full_name: Mapped[str] = mapped_column(String(100), nullable=False, comment="全名")
    bio: Mapped[str | None] = mapped_column(String(255), comment="个人简介")

    # 外键指向 User 表
    # unique=True 唯一性约束，保证1对1唯一性关系
    user_id: Mapped[int] = mapped_column(
        Integer, ForeignKey("users.id"), unique=True, comment="关联用户ID"
    )

    # 定义一对一关系
    user: Mapped["User"] = relationship("User", back_populates="profile")


# 数据库创建表
Base.metadata.create_all(engine)
```



## 添加数据

```python
import os
# 修改项目根目录
os.chdir(os.path.dirname(__file__))
from sqlalchemy import insert, select
from init.create_table import engine, User, Profile
from init.get_db import db_getter


with db_getter() as session:
    # 声明式(Declarative) - ORM方式 插入数据 （对象实例化）
    userments = [
        User(username="总经办",),
        User(username="研发中心",),
        User(username="产品部",),
    ]

    new_profile = [
        Profile(full_name="总经办11", user=userments[0]),
        Profile(full_name="研发中心22", user=userments[1]),
        Profile(full_name="产品部33", user=userments[2]),
    ]

    # 第一种方式 
    # 创建 profile 表同时会自动创建 user 表 ，可批量可单个
    session.add_all(new_profile)
    # 第二种方式
    # 通过查询已存在表创建，可批量可单个
    session.add_all(userments)
    session.execute(
        insert(Profile).values(
            [
                {
                    'full_name': "总经办11",
                    'user_id': select(User.id).where(User.username == '总经办'),
                }
            ]
        )
    )

```



## 查询数据

## 更新数据

## 删除数据

# 五、关联表之一对多

## 表定义

## 添加数据

## 查询数据

## 更新数据

## 删除数据

# 六、关联表之多对多

## 表定义

## 添加数据

## 查询数据

## 更新数据

## 删除数据

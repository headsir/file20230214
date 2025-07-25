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

### 直连方式

- 低级SQL核心接口，接近原生SQL
- 需要手动处理结果集映射
- 直接执行SQL语句

```python
from sqlalchemy import create_engine, text

# 创建引擎 echo=True 会打印SQL语句,方便调试,生产环境建议关闭

# SQLite连接方式，默认使用sqlite3引擎，无需安装第三方库，直接使用
# db文件不存在会自动创建
# engine = create_engine('sqlite:///test.db', echo=True)

# MYSQL数据库默认MySQLdb引擎，如需使用其它引擎，需要通过【mysql+引擎】指明，推荐pymysql
engine = create_engine('mysql+pymysql://root:qazwsx@localhost:3306/塔租费用', echo=True)

# 创建连接
conn = engine.connect()
# 查询语句
query = text('select * from `地市确认单金额` limit 1;')
# 执行查询
result = conn.execute(query)
# 获取结果集
# fetchall() 获取所有结果,返回一个列表
# fetchone() 获取一条结果,返回一个元组
# fetchmany(n) 获取n条结果返回一个列表
get_data = result.fetchall()
# 关闭连接
conn.close()
# 关闭引擎
engine.dispose()
```

### ORM方式

- 高级ORM接口，面向对象操作
- 自动管理对象状态（如脏数据检测）

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import Session
from contextlib import contextmanager

# 创建引擎（连接数据库）
engine = create_engine("sqlite:///mydatabase.db", echo=True)

# @contextmanager 是 Python 标准库 contextlib 模块提供的一个装饰器，
# 用于将一个生成器函数转换为上下文管理器。它的主要作用是简化上下文管理器的创建过程。
@contextmanager
def db_getter():
    """
    获取数据库连接
    """
    # 创建会话
    with Session(engine) as session:
        # 创建一个新的事务，半自动 commit
        with session.begin():
            # 事务提交
            yield session
```

创建session有2种方式：

- Session(engine)

- sessionmaker(bind=engine) 推荐这种

  通过sessionmake方法创建一个Session工厂，然后在调用工厂的方法来实例化一个Session对象



# 二、创建表

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

使用的是ORM方式连接数据库，会自动提交数据，此处省略session.commit()

## 添加数据

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

## 查询数据

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

## 更新数据

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



## 删除数据

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


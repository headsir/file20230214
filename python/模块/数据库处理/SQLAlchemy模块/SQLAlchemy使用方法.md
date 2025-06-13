SQLAlchemy使用方法

# 一、SQLAlchemy介绍

Python中最有名的ORM架构就是SQLAlchemy，我们主要就是来学习SQLAlchemy的使用

## 1.1 安装环境：

```undefined
pip install SQLAlchemy
```

## 1.2 安装mysql

```css
yum install mysql-server mysql
service mysqld restart
sysctmctl restart mysql.service
```

## 1.3 创建数据库

```
create database sqlalchemy;
```

## 1.4 授权

```
GRANT ALL PRIVILEGES ON *.* TO 'fxq'@'%' IDENTIFIED BY ‘123456’;
```

## 1.5 初始化连接

```
from sqlalchemy import create_engine
engine = create_engine('mysql://fxq:123456@192.168.100.101/sqlalchemy', echo=True)

echo参数为True时，会显示每条执行的SQL语句，可以关闭，
 create_engine()返回一个Engine的实例，并且它表示通过数据库语法处理细节的核心接口，在这种情况下，数据库语法将会被解释成python的类方法。
 解释说明：
 [mysql://fxq:123456@192.168.100.101/sqlalchemy](mysql://fxq:123456@192.168.100.101/sqlalchemy)
 mysql:  指定是哪种数据库连接
 第一个fxq： 用户名
 123456： fxq用户对应的密码
 192.168.100.101： 数据库的ip
 sqlalchemy： 数据库需要连接库的名字
```

## 1.6 创建表格

### 1.主要是通过sql语句来创建表格：

```
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

sql = '''create table student(
    id int not null primary key,
    name varchar(50),
    age int,
    address varchar(100));
'''

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
conn = engine.connect()
conn.execute(sql)
engine.connect() #表示获取到数据库连接。类似我们在MySQLdb中游标course的作用
```

###  2.通过ORM方式创建表格

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/10 21:58
# @Author  : Feng Xiaoqing
# @File    : demo2.py
# @Function: -----------

from sqlalchemy import create_engine, MetaData, Table, Column, Integer, String

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
metadata = MetaData(engine)


student = Table('student', metadata,
            Column('id', Integer, primary_key=True),
            Column('name', String(50), ),
            Column('age', Integer),
            Column('address', String(10)),
)

metadata.create_all(engine)
```

以上方式都可以创建数据库表
 MetaData类主要用于保存表结构，连接字符串等数据，是一个多表共享的对象
 metadata = MetaData(engine)    #绑定一个数据源的metadata
 metadata.create_all(engine)         #是来创建表，这个操作是安全的操作，会先判断表是否存在。

### 3. Table类

构造函数：

```
Table.__init__(self, name, metadata,*args, **kwargs)
```

name    表名
 metadata      共享的元数据
 *args Column 是列定义，详见下一节
 下面是可变参数 **kwargs 定义
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

### 4. Column类

构造函数：

```
Column.__init__(self,  name,  type_,  *args,  **kwargs)
```

1、name 列名
 2、type_ 类型，更多类型 sqlalchemy.types
 3、*args Constraint（约束）,  ForeignKey（外键）,  ColumnDefault（默认）, Sequenceobjects（序列）定义
 4、key 列名的别名，默认None
 下面是可变参数 **kwargs
 5、primary_key 如果为True，则是主键
 6、nullable 是否可为Null，默认是True
 7、default 默认值，默认是None
 8、index 是否是索引，默认是True
 9、unique 是否唯一键，默认是False
 10、onupdate 指定一个更新时候的值，这个操作是定义在SQLAlchemy中，不是在数据库里的，当更新一条数据时设置，大部分用于updateTime这类字段
 11、autoincrement 设置为整型自动增长，只有没有默认值，并且是Integer类型，默认是True
 12、quote 如果列明是关键字，则强制转义，默认False

### 5.创建会话：

说到数据库，就离不开Session。Session的主要目的是建立与数据库的会话，它维护你加载和关联的所有数据库对象。它是数据库查询（Query）的一个入口。
 在Sqlalchemy中，数据库的查询操作是通过Query对象来实现的。而Session提供了创建Query对象的接口。
 Query对象返回的结果是一组同一映射（Identity Map）对象组成的集合。事实上，集合中的一个对象，对应于数据库表中的一行（即一条记录）。所谓同一映射，是指每个对象有一个唯一的ID。如果两个对象（的引用）ID相同，则认为它们对应的是相同的对象。
 要完成数据库查询，就需要建立与数据库的连接。这就需要用到Engine对象。一个Engine可能是关联一个Session对象，也可能关联一个数据库表。
 当然Session最重要的功能还是实现原子操作。
 ORM通过session与数据库建立连接进行通信，如下所示：

```
from sqlalchemy.orm import sessionmaker

DBSession = sessionmaker(bind=engine)
session = DBSession()
```

通过sessionmake方法创建一个Session工厂，然后在调用工厂的方法来实例化一个Session对象。

## 1.7 添加数据

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/10 20:58
# @Author  : Feng Xiaoqing
# @File    : demo1.py
# @Function: -----------

from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker


engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBsession = sessionmaker(bind=engine)
session = DBsession()

Base = declarative_base()

class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(100))
    age = Column(Integer)
    address = Column(String(100))

student1 = Student(id=1001, name='ling', age=25, address="beijing")
student2 = Student(id=1002, name='molin', age=18, address="jiangxi")
student3 = Student(id=1003, name='karl', age=16, address="suzhou")
# 也可以通过字典
session.add_all([student1, student2, student3])
session.commit()
session.close()

```



## 1.8 查询

查询是这个里面最为复杂，最为繁琐的一个步骤。
 通过Session的query()方法创建一个查询对象。这个函数的参数数量是可变的，参数可以是任何类或者是类的描述的集合。下面来看一个例子：

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter_by(name="fengxiaoqing2").first()
print(my_stdent)
```

此时我们看到的输出结果是这样的：

```
<__main__.Student object at 0x032745F0>
```

前面我们在赋值的时候，我们可以通过实例化一个对象，然后直接映射到数据库中，那我们在查询出来的数据sqlalchemy直接给映射成一个对象了（或者是每个元素为这种对象的列表），对象和我们创建表时候的class是一致的，我们就也可以直接通过对象的属性就可以直接调用就可以了。

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter_by(name="fengxiaoqing2").first()
print(my_stdent.id,my_stdent.name,my_stdent.age,my_stdent.address)
```

结果：

```
1000311 fengxiaoqing2 182 chengde
```

### filter()  过滤表的条件

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()


my_stdent = session.query(Student).filter(Student.name.like("%feng%"))
print(my_stdent)
```

结果：

```
SELECT student.id AS student_id, student.name AS student_name, student.age AS student_age, student.address AS student_address 
FROM student 
WHERE student.name LIKE %s
```

根据结果，我们可以看出来
 filter_by最后的结果就是一个sql语句，我们排错的时候就可以通过这个来排查我们sql是否正确。
 以下的这些过滤操作都可以在filter函数中使用：

```
equals:
query(Student).filter(Student.id == 10001)
not equals:
query(Student).filter(Student.id != 100)
LIKE:
query(Student).filter(Student.name.like(“%feng%”))

IN:
query(Student).filter(Student.name.in_(['feng', 'xiao', 'qing']))
not in
query(Student).filter(~Student.name.in_(['feng', 'xiao', 'qing']))

AND:
from sqlalchemy import and_
query(Student).filter(and_(Student.name == 'fengxiaoqing', Student.id ==10001))

或者
query(Student).filter(Student.name == 'fengxiaoqing').filter(Student.address == 'chengde')

OR:
from sqlalchemy import or_
query.filter(or_(Student.name == 'fengxiaoqing', Student.age ==18))
```

### 返回列表(List)和单项(Scalar)

all()  返回一个列表

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter(Student.name.like("%feng%")).all()
print(my_stdent)
```

结果：

```
[<__main__.Student object at 0x031405B0>, <__main__.Student object at 0x030FCA70>, <__main__.Student object at 0x031405F0>]
```

可以通过遍历列表来获取每个对象。
 one()    返回且仅返回一个查询结果。当结果的数量不足一个或者多于一个时会报错。
 把上面的all改成one就报错了。
 first()    返回至多一个结果，而且以单项形式，而不是只有一个元素的tuple形式返回这个结果.

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 21:49
# @Author  : Feng Xiaoqing
# @File    : search.py
# @Function: -----------
from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter(Student.name.like("%feng%")).first()
print(my_stdent)
```

结果：

```
<__main__.Student object at 0x030A3610>
```

### filter()和filter_by()的区别：

Filter：  可以像写 sql 的 where 条件那样写 > < 等条件，但引用列名时，需要通过 类名.属性名 的方式。
 filter_by：  可以使用 python 的正常参数传递方法传递条件，指定列名时，不需要额外指定类名。，参数名对应名类中的属性名，但似乎不能使用 > < 等条件。

当使用filter的时候条件之间是使用“=="，fitler_by使用的是"="。

```
user1 = session.query(User).filter_by(id=1).first()
user1 = session.query(User).filter(User.id==1).first()
```

filter不支持组合查询，只能连续调用filter来变相实现。
 而filter_by的参数是**kwargs，直接支持组合查询。
 比如：

```
q = sess.query(IS).filter(IS.node == node and IS.password == password).all()
```

## 1.9 更新

更新就是查出来，直接更改就可以了

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

my_stdent = session.query(Student).filter(Student.id == 1002).first()
my_stdent.name = "fengxiaoqing"
my_stdent.address = "chengde"
session.commit()
student1 = session.query(Student).filter(Student.id == 1002).first()
print(student1.name, student1.address)
```

结果：

```
MariaDB [sqlalchemy]> select * from student;
+---------+---------------+------+---------+
| id      | name          | age  | address |
+---------+---------------+------+---------+
|    1002 | molin         |   18 | jiangxi |
|    1003 | karl          |   16 | suzhou  |
|  100011 | fengxiaoqing  |   18 | chengde |
|  100021 | fengxiaqing1  |  181 | chengde |
| 1000111 | fengxiaoqing  |   18 | chengde |
| 1000211 | fengxiaqing1  |  181 | chengde |
| 1000311 | fengxiaoqing2 |  182 | chengde |
+---------+---------------+------+---------+
7 rows in set (0.00 sec)

MariaDB [sqlalchemy]> select * from student;
+---------+---------------+------+---------+
| id      | name          | age  | address |
+---------+---------------+------+---------+
|    1002 | fengxiaoqing  |   18 | chengde |
|    1003 | karl          |   16 | suzhou  |
|  100011 | fengxiaoqing  |   18 | chengde |
|  100021 | fengxiaqing1  |  181 | chengde |
| 1000111 | fengxiaoqing  |   18 | chengde |
| 1000211 | fengxiaqing1  |  181 | chengde |
| 1000311 | fengxiaoqing2 |  182 | chengde |
+---------+---------------+------+---------+
7 rows in set (0.00 sec)

MariaDB [sqlalchemy]> 
```

## 1.10 删除

删除其实也是跟查询相关的，直接查出来，调用delete()方法直接就可以删除掉。

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()


session.query(Student).filter(Student.id == 1001).delete()
session.commit()
session.close()
```

## 1.11 统计、分组、排序

### 1.统计count()

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

print(session.query(Student).filter(Student.name.like("%feng%")).count())
```

### 2.分组 group_by()

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

std_group_by = session.query(Student).group_by(Student.age)
print(std_group_by)
```

结果的sql语句如下：

```
SELECT student.id AS student_id, student.name AS student_name, student.age AS student_age, student.address AS student_address 
FROM student GROUP BY student.age
```

### 3.排序 order_by()     反序在order_by里面用desc()方法

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/5/11 22:27
# @Author  : Feng Xiaoqing
# @File    : update.py
# @Function: -----------

from sqlalchemy import Column
from sqlalchemy import Integer
from sqlalchemy import String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()
class Student(Base):
    __tablename__ = 'student'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    address = Column(String(100))

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
DBSession = sessionmaker(bind=engine)
session = DBSession()

std_ord_desc = session.query(Student).filter(Student.name.like("%feng%")).order_by(Student.id.desc()).all()
for i in std_ord_desc:
  print(i.id)
```

###### 结果：

```
1000311
1000211
1000111
100021
100011
1002
```

> 参考资料

```text
    https://www.cnblogs.com/luckzack/p/18182122
```

# Alembic 的应用

Alembic 使用 SQLAlchemy 作为数据库引擎，为关系型数据提供创建、管理、更改和调用的管理脚本，协助开发和运维人员在系统上线后对数据库进行在线管理。

同任何 Python 扩展库一样，我们可以通过 pip 来快速的安装最新的稳定版 Alembic 扩展库 `pip install alembic`。

```cmd
pip install alembic
```

## 创建 Alembic 迁移环境

在使用 Alembic 之前需要先建立一个 Alembic 脚本环境，通过在工程目录下输入 `alembic init alembic` 命令可以快速在应用程序中建立 Alembic 脚本环境，当在命令行看到以下输出时，表示 alembic 脚本环境创建完成。

```cmd
alembic init alembic

Creating directory 'D:\\数据库\\202409项目\\03模块使用说明\\05_ORM_SQLAlchemy\\Alembic 的应用\\alembic' ...  done
Creating directory 'D:\\数据库\\202409项目\\03模块使用说明\\05_ORM_SQLAlchemy\\Alembic 的应用\\alembic\\versions' ...  done
Generating D:\数据库\202409项目\03模块使用说明\05_ORM_SQLAlchemy\Alembic 的应用\alembic.ini ...  done
Generating D:\数据库\202409项目\03模块使用说明\05_ORM_SQLAlchemy\Alembic 的应用\alembic\env.py ...  done
Generating D:\数据库\202409项目\03模块使用说明\05_ORM_SQLAlchemy\Alembic 的应用\alembic\README ...  done
Generating D:\数据库\202409项目\03模块使用说明\05_ORM_SQLAlchemy\Alembic 的应用\alembic\script.py.mako ...  done
Please edit configuration/connection/logging settings in 'D:\\数据库\\202409项目\\03模块使用说明\\05_ORM_SQLAlchemy\\Alembic 的应用\\alembic.ini' before proceeding.
```

你可以通过 -t 选项来选择一个初始化的模板，Alembic 目前支持三个初始化模板「通过 alembic list_templates 可以查看支持的模板类型」，默认情况下使用的是通用模板，在大多数情况下使用通用模板即可。

```text
Available templates:

async - Generic single-database configuration with an async dbapi.
generic - Generic single-database configuration.
multidb - Rudimentary multi-database configuration.

Templates are used via the 'init' command, e.g.:

  alembic init --template generic ./scripts
```

生成的迁移脚本目录如下：

```text
├── alembic
│   ├── README
│   ├── env.py
│   ├── script.py.mako
│   └── versions
```

- alembic 目录：迁移脚本的根目录，放置在应用程序的根目录下，可以设置为任意名称。在多数据库应用程序可以为每个数据库单独设置一个 Alembic 脚本目录。
- README 文件：说明文件，初始化完成时没有什么意义。
- env.py 文件：一个 python 文件，在调用 Alembic 命令时该脚本文件运行。
- script.py.mako 文件：是一个 mako 模板文件，用于生成新的迁移脚本文件。
- versions 目录：用于存放各个版本的迁移脚本。初始情况下为空目录，通过 revision 命令可以生成新的迁移脚本。

```text
alembic 会在你的应用程序的根目录下生成一个 alembic.ini 的配置文件，在开始任何的操作之前需要先修改该文件中的 sqlalchemy.url 指向你自己的数据库地址。

比如：sqlalchemy.url = mysql+pymysql://temp:qazwsx@localhost/temp
```

## 生成迁移脚本

当 Alembic 配置环境创建完成后，可以通过 Alembic 的子命令 revision 来生成新的迁移脚本。

```cmd
alembic revision -m "first create script"

Generating D:\数据库\202409项目\03模块使用说明\05_ORM_SQLAlchemy\Alembic 的应用\alembic\versions\39449bfacab3_first_create_script.py ...  done
```

初始的迁移脚本中并没有实际有效的内容，相当于一个空白的模板文件「增加了版本信息」。如果对整改工程的数据表进行修改后，再次运行 revision 子命令可以看到新生成的脚本文件中的内容增加了我们对数据表的改变内容。

```cmd
alembic revision -m "add create date in user table"

Generating D:\数据库\202409项目\03模块使用说明\05_ORM_SQLAlchemy\Alembic 的应用\alembic\versions\293d5ef8f65f_add_create_date_in_user_table.py ...  done
```

此时在 alembic 文件夹中可以看到以下文件:

```text
alembic
├── README
├── env.py
├── script.py.mako
└── versions
    ├── 3602707b314b_first_create_script.py
    └── eac6fb06ced5_add_create_date_in_user_table.py
```

可以看到在 versions 目录中生成了两个迁移脚本文件，但是此时的迁移脚本文件中没有任何的有效代码，文件内容如下:
add create date in user table.py

```text
Revision ID: eac6fb06ced5
Revises: 3602707b314b
Create Date: 2019-11-03 06:54:23.862575
"""
from alembic import op
import sqlalchemy as sa
# revision identifiers, used by Alembic.
revision = 'eac6fb06ced5'
down_revision = '3602707b314b'
branch_labels = None
depends_on = None
def upgrade():
    pass
def downgrade():
    pass
```

在该文件中制定了当前版本号 revision 和父版本号 down_revision ，以及相应的升级操作函数 upgrade 和降级操作函数 dwongrade。在 upgrade 和 dwongrade 函数中通过相应的 API 来操作 op 和 sa 对象来完成对数据库的修改，以下代码完成了在数据库中新增一个 account 数据表的功能。
注意：`alembic upgrade head` 可升级最新版本

```text
def upgrade():
    op.create_table(
        'account',
        sa.Column('id', sa.Integer, primary_key=True),
        sa.Column('name', sa.String(50), nullable=False),
        sa.Column('description', sa.Unicode(200)),
    )
def downgrade():
    op.drop_table('account')
```

每次编写完代码还需要手动编写迁移脚本这并不是程序员所需要的，幸运的是 Alembic 的开发者为程序员提供了更美好的操作「自动生成迁移脚本」。自动生成迁移脚本无需考虑数据库相关操作，只需完成 ROM 中相关类的编写即可，通过 Alembic 命令即可在数据库中自动完成数据表的生成和更新。在 Alembic 中通过 revision 子命令的 --autogrenerate 选项参数来生成自动迁移脚本。

在使用自动生成命令之前，需要在 env.py 文件中修改 target_metadata 配置使其指向应用程序中的元数据对象。
env.py

```python
# add your model's MetaData object here
# for 'autogenerate' support
# from myapp import mymodel
# target_metadata = mymodel.Base.metadata
# target_metadata = None

# 引入数据库基类
import os, sys
sys.path.append(os.path.dirname(os.path.abspath(__file__)))
import module
target_metadata = module.Base.metadata
```

module.py

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

engine = create_engine('mysql+pymysql://temp:qazwsx@localhost/temp')
Base = declarative_base()


class Student(Base):  # Student类继承自Base
    __tablename__ = 'student2'
    id = Column(Integer, primary_key=True)
    name = Column(String(100))
    age = Column(Integer)
    address = Column(String(100))
```

在 temp 数据库中新增 student2 数据表，然后使用自动生成迁移脚本命令，查看我们的配置是否完成。运行命令`alembic revision --autogenerate`后可以看到以下信息：

```text
INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
INFO  [alembic.autogenerate.compare] Detected added table 'student2'
Generating D:\数据库\202409项目\03模块使用说明\05_ORM_SQLAlchemy\Alembic 的应用\alembic\versions\67fd988f79d5_.py ...  done
```

生成的迁移脚本文件内容如下：

```python
def upgrade() -> None:
    # ### commands auto generated by Alembic - please adjust! ###
    op.create_table(
        'student2',
        sa.Column('id', sa.Integer(), nullable=False),
        sa.Column('name', sa.String(length=100), nullable=True),
        sa.Column('age', sa.Integer(), nullable=True),
        sa.Column('address', sa.String(length=100), nullable=True),
        sa.PrimaryKeyConstraint('id'),
    )
    # ### end Alembic commands ###


def downgrade() -> None:
    # ### commands auto generated by Alembic - please adjust! ###
    op.drop_table('student2')
    # ### end Alembic commands ###
```

## 变更数据库

Alembic 最重要的功能是自动完成数据库的迁移「变更」，所做的配置以及生成的脚本文件都是为数据的迁移做准备的，数据库的迁移主要用到 upgrade 和 downgrade 子命令。

数据看的变更主要用到以下命令：

- **alembic upgrade head**：将数据库升级到最新版本。
- **alembic downgrade base**：将数据库降级到最初版本。
- **alembic upgrade <version>**：将数据库升级到指定版本。
- **alembic downgrade <version>**：将数据库降级到指定版本。
- **alembic upgrade +2**：相对升级，将数据库升级到当前版本后的两个版本。
- **alembic downgrade +2**：相对降级，将数据库降级到当前版本前的两个版本。
- **alembic stamp head**：命令的作用是将数据库标记为已应用最新迁移版本

以上所有的升降级方式都是在线方式实时更新数据库文件，实际环境中总会存在一些环境无法在线升级，Alembic 提供了生成 SQL 脚本的形式，已提供离线升降级的功能。

```text
alembic upgrade <version> --sql > migration.sql

或

alembic downgrade <version> --sql > migration.sql
```

随着项目的进展，项目下不可避免的会生成很多版本的迁移脚本，此时可以使用 current 来查看线上数据库处于什么版本，也可以通过 history 来查看项目目录中的迁移脚本信息。

```tex
alembic current

INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
822973f596d5 (head)

alembic history

67fd988f79d5 -> 822973f596d5 (head), add create date in user table
<base> -> 67fd988f79d5, empty message
```


# 一、**SQLAlchemy**介绍

ORM工具，全称Object Relational Mapping（对象关系映射）。

它可以将你的代码从底层数据库及其相关的SQL特性中抽象出来。特点是操纵Python对象而不是SQL查询，也就是在代码层面考虑的是对象，而不是SQL，体现的是一种程序化思维，这样使得Python程序更加简洁易读。

# 二、实现数据库和excel之间的导入导出

## 2.1 数据库->Excel

```python
from sqlalchemy import create_engine
import pandas as pd

# 创建数据库连接,mysql用户名是root，密码是211314，本地的数据库服务是localhost,数据库的名称hong
engine = create_engine('mysql+pymysql://root:211314@localhost/hong')

# 读取mysql数据,使用pandas的read_sql()查询mysql表department中的数据
db = pd.read_sql(sql='select * from hong.department', con=engine)

# 导出数据到excel,通过pandas的to_excel()写到本地
db.to_excel('部门数据.xlsx')
```

## 2.2 Excel->数据库

```python
from sqlalchemy import create_engine
import pandas as pd

# 创建数据库连接
engine = create_engine('mysql+pymysql://root:211314@localhost/hong')

# 读取xlsx文件,使用pandas的read_excel()读取本地文件
df = pd.read_excel('模拟数据.xlsx')

# 导入到mysql数据库,使用pandas的to_sql()方法将读取到的数据写入到mysql中
df.to_sql(name='test_data', con=engine, index=False, if_exists='replace')
```


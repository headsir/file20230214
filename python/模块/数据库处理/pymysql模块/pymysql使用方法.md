# 一、pymysql封装

## 第一版：

```
# coding = utf-8

import pymysql


class Model(object):
    def __init__(self, username='root', password='123456', database='demo',
                 port=3306, host='localhost'):
        """

        :param username: MySQL 用户名
        :param password: MySQL 密码
        :param database: 数据库名称
        :param port: 端口号
        :param host: 主机名或 IP 地址
        cursorclass：统一将查询返回的数据格式转为 列表嵌套字典 [{},{}...]
        """
        # 创建连接
        self.connection = pymysql.connect(user=username, password=password, database=database,
                                          port=port, host=host, cursorclass=pymysql.cursors.DictCursor)
        # 创建游标
        self.cursor = self.connection.cursor()

    # 查询所有数据
    def fetchall(self, sql):
        try:
            self.__execute(sql)
            return self.cursor.fetchall()
        except Exception as error:
            print(error)

    # 查询多条数据
    def fetchmany(self, sql, size=1):
        try:
            self.__execute(sql)
            return self.cursor.fetchmany(size)
        except Exception as error:
            print(error)

    # 查询一条数据
    def fetchone(self, sql):
        try:
            self.__execute(sql)
            return self.cursor.fetchone()
        except Exception as error:
            print(error)

    # 增删改的方法
    def change(self, sql):
        try:
            self.__execute(sql)
            self.connection.commit()
        except Exception as error:
            print(error)

    # 执行的私有方法
    def __execute(self, sql):
        self.cursor.execute(sql)

    # 关闭连接和游标
    def __del__(self):
        self.connection.close()
        self.cursor.close()

```

## 第二版：

优化mysql配置参数及批量添加数据

```sql
# coding = utf-8

import pymysql
from loguru import logger
"""
# python 操作mysql数据库的方法封装
"""


class Model(object):
    """
    操作mysql数据库的方法封装
    """
    # def __init__(self, username='root', password='qazwsx', database='试验库',
    #              port=3306, host='localhost'):
    def __init__(self, dbinfo):
        # 创建连接2种方法
        # 第一种方法
        """
        数据库相关信息单个传入，已弃用
        :param username: MySQL 用户名
        :param password: MySQL 密码
        :param database: 数据库名称
        :param port: 端口号
        :param host: 主机名或 IP 地址
        cursorclass：统一将查询返回的数据格式转为 列表嵌套字典 [{},{}...]
        """
        # self.connection = pymysql.connect(user=username, password=password, database=database,
        #                                   port=port, host=host, cursorclass=pymysql.cursors.DictCursor)

        # 第二种方法
        """
        数据库相关信息以字典的方式传入
        :param dbinfo: 数据库相关信息的字典
        """
        # %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
        self.connection = pymysql.connect(**dbinfo, cursorclass=pymysql.cursors.DictCursor)
        # %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
        logger.debug("连接数据库为：mysql://{0}:******@{1}:{2}/{3}".format(
            dbinfo["user"], dbinfo["host"], dbinfo["port"], dbinfo["database"]))

        # 创建游标
        self.cursor = self.connection.cursor()

    # 查询所有数据
    def fetchall(self, sql):
        try:
            self.__execute(sql)
            return self.cursor.fetchall()
        except Exception as error:
            print(error)

    # 查询多条数据
    def fetchmany(self, sql, size=1):
        try:
            self.__execute(sql)
            return self.cursor.fetchmany(size)
        except Exception as error:
            print(error)

    # 查询一条数据
    def fetchone(self, sql):
        try:
            self.__execute(sql)
            return self.cursor.fetchone()
        except Exception as error:
            print(error)

    # 增删改的方法
    def change(self, sql,data=None):
        try:
            self.__execute(sql,data)
            self.connection.commit()
            return "执行成功"
        except Exception as error:
            print(error)
            # return error

    # 执行的私有方法 %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
    def __execute(self, sql, data=None):
        # data为空，是执行删除、修改或单个数据添加的SQL语句
        if data == None:
            self.cursor.execute(sql)
        # data 不为空，是执行添加多个数据的SQL语句
        else:
            # 一次执行可以操作多个数据
            self.cursor.executemany(sql, data)
    # %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

    # 关闭连接和游标
    def __del__(self):
        self.cursor.close()
        self.connection.close()

# 数据批量导入数据的方法
def data_export_mysql(data, mysql, msql_sheet):
    """
    数据批量导入数据的方法
    :param data: 数据，格式 DataFrame
    :param mysql: 数据库实例
    :param msql_sheet: 数据库【表名】
    :return:
    """
    # 构造SQL语句
    data_columns = "`,`".join(list(data.columns)).replace("%", "%%")
    # 构造SQL语句value值
    data_list = [tuple(i) for i in data.fillna("/").values]
    # 计算VALUE值个数
    s_count_data = len(data_list[0]) * "%s,"
    # 构造SQL语句
    insert_sql_data = f"insert into `{msql_sheet}`(`{data_columns}`) values ({s_count_data[:-1]});"
    # 数据导入数据库
    mysql_rulse = mysql.change(insert_sql_data, data_list)
    if mysql_rulse == "执行成功":
        logger.debug(f"【{msql_sheet}】数据库执行情况：{mysql_rulse},添加{len(data_list)}条数据")
    else:
        logger.debug(mysql_rulse)

    return
```



```
# 构造SQL语句列
traffic_columns = "`,`".join(list(traffic.columns))
# 构造SQL语句value值
traffic_list = [tuple(i) for i in traffic.fillna("/").values]
# 计算VALUE值个数
s_count_traffic = len(traffic_list[0]) * "%s,"

备注：字段出现 % 需使用 %%
```

## 第三版：

- 添加了相关函数说明及使用方法

- 新建了获取数据库表字段函数

- 取消了空值替换处理，建议放到导入数据之前处理

  ```python
  data = data.replace("", None)  # 字符串 替换
  data = data.replace(np.nan, None)  # 数字 替换
  ```

```python
# coding = utf-8

"""
# python 操作mysql数据库的方法封装
更新日期：20231211
"""
import pymysql

from loguru import logger


class Model(object):
    """
    操作mysql数据库的方法封装
    """

    # def __init__(self, username='root', password='qazwsx', database='试验库',
    #              port=3306, host='localhost'):
    def __init__(self, dbinfo):
        # 创建连接2种方法
        # 第一种方法
        # """
        # 数据库相关信息单个传入，已弃用
        # :param username: MySQL 用户名
        # :param password: MySQL 密码
        # :param database: 数据库名称
        # :param port: 端口号
        # :param host: 主机名或 IP 地址
        # cursorclass：统一将查询返回的数据格式转为 列表嵌套字典 [{},{}...]
        # """
        # self.connection = pymysql.connect(user=username, password=password, database=database,
        #                                   port=port, host=host, cursorclass=pymysql.cursors.DictCursor)

        # 第二种方法
        """
        数据库相关信息以字典的方式传入
        :param dbinfo: 数据库相关信息的字典,**将字典参数转为关键字参数对
        """
        # %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
        self.connection = pymysql.connect(**dbinfo, cursorclass=pymysql.cursors.DictCursor)
        # %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
        logger.debug("连接数据库为：mysql://{0}:******@{1}:{2}/{3}".format(
            dbinfo["user"], dbinfo["host"], dbinfo["port"], dbinfo["database"]))
        # 创建游标
        self.cursor = self.connection.cursor()

    # 查询所有数据
    def fetchall(self, query, params=None):
        """
        Mysql查询所有数据
        :param query: SQL查询语句，必须参数。
        :param params: SQL查询语句参数，可选参数。
        :return:返回二维元组，如((‘id’,‘title’),)
        """
        """
        函数使用说明：
            下面为SQL查询数据表字段语句：
            select COLUMN_NAME from information_schema.COLUMNS
            where table_name = %s AND table_schema = %s;
            params = (table, database)
            # 数据表字段获取
            [i["COLUMN_NAME"] for i in mysql.fetchall(sql语句, 参数)]
        """
        try:
            # 执行SQL语句
            self.cursor.execute(query, params)
            # 获取记录列表
            return self.cursor.fetchall()
        except Exception as error:
            raise error

    # 查询多条数据
    def fetchmany(self, query, params=None, size=None):
        """
        Mysql查询前 n 行数据
        :param query: SQL查询语句，必须参数。
        :param params: SQL查询语句参数，可选参数。
        :param size: 设置查询数量，可选参数,默认返回一条数据。
        :return:返回二维元组，如((‘id’,‘title’),(‘id’,‘title’),)
        """
        """
        函数使用说明：
            下面为SQL查询数据表字段语句：
            select COLUMN_NAME from information_schema.COLUMNS
            where table_name = %s AND table_schema = %s;
            params = (table, database)
            # 数据表字段获取
            [i["COLUMN_NAME"] for i in mysql.fetchmany(sql语句, 参数, 返回结果数量 )]
        """
        try:
            self.cursor.execute(query, params)
            return self.cursor.fetchmany(size)
        except Exception as error:
            raise error

    # 查询一条数据
    def fetchone(self, query, params=None):
        """
        Mysql查询最上面的第一条结果
        :param query: SQL查询语句，必须参数。
        :param params: SQL查询语句参数，可选参数。
        :return:返回单个的元组
        """
        """
        函数使用说明：
            下面为SQL查询数据表字段语句：
            select COLUMN_NAME from information_schema.COLUMNS
            where table_name = %s AND table_schema = %s;
            params = (table, database)
            # 数据表字段获取
            mysql.fetchone(sql, params)
        """
        try:
            self.cursor.execute(query, params)
            return self.cursor.fetchone()
        except Exception as error:
            raise error

    # 增删改的方法
    def change(self, query, params=None, many=False):
        """
            Mysql更删改的方法
        :param query: SQL查询语句，必须参数。
        :param params: SQL查询语句参数，可选参数。
        :param many: 设置执行单条数据还是批量数据，可选参数,默认执行单条条数据。
        :return: 受影响的行数
        """
        """
        使用说明：
        删除单条数据SQL语句：
            DELETE FROM `铁塔费用_原始数据v1_copy1` AS d
            WHERE d.序号 = %s
            mysql.change(SQL语句,params=("821874")
        批量添加数据SQL语句：：
            INSERT INTO `铁塔费用_原始数据v1_copy1` (`字段1`,`字段2`) VALUES (%s,%s)
            params=[[字段1的值，字段2的值],[字段1的值，字段2的值]]
            mysql.change(SQL语句, params,many=True)
        批量更新数据SQL语句：
            table_name="student"
            UPdate `{table_name}` AS d set d.subjectid=%s WHERE d.name = %s    
            params=[(13, "John"), (11, "Martin")]
            mysql.change(SQL语句, params,many=True)
        """
        try:
            # many=True，是执行处理多个数据的SQL语句
            if many:
                # 一次执行可以操作多个数据
                res = self.cursor.executemany(query, params)
            # many=False,执行删除、修改或单个数据添加的SQL语句
            else:
                res = self.cursor.execute(query, params)
            self.connection.commit()
            return res
        except Exception as error:
            raise error

    # 关闭连接和游标
    def close(self):
        self.cursor.close()
        self.connection.close()


# 数据批量导入数据库的方法
def data_export_mysql(data, mysql, msql_sheet):
    """
    数据批量导入数据的方法,空值请导入前处理完成，否则报错
    :param data: 数据，格式 DataFrame
    :param mysql: 数据库实例
    :param msql_sheet: 数据库【表名】
    :return:
    """
    """
    使用说明：
        1、空值处理建议：
            data = data.replace("", None)  # 字符串 替换
            data = data.replace(np.nan, None)  # 数字 替换
    """
    # 构造SQL语句
    data_columns = "`,`".join(list(data.columns)).replace("%", "%%")
    # 构造SQL语句value值,将DataFrame类型转为[[],[],[]]
    data_list = data.values.tolist()
    # 计算VALUE值个数
    s_count_data = len(data_list[0]) * "%s,"
    # 构造SQL语句
    insert_sql_data = f"INSERT INTO `{msql_sheet}` (`{data_columns}`) VALUES ({s_count_data[:-1]});"
    # 数据导入数据库
    res = mysql.change(query=insert_sql_data, params=data_list, many=True)
    logger.info(f"数据库【{msql_sheet}】：添加{res}条数据")
    return res


# 获取Mysql数据库某个表所有字段。
def getMysqlField(mysql, database, table):
    """
        获取Mysql数据库某个表所有字段。
    :param mysql: Mysql 对象
    :param database: 数据库名字
    :param table: 表名字
    :return:返回数据表所有字段
    """
    """
    使用说明：
    1、获取Mysql数据库【database】表【表名】，所有字段
    columns_list = getMysqlField(
        mysql=mysql,
        database="database",
        table=“表名”
    )
    2、如果哪个字段不需要通过列表推导式将其排除
    removeList = ['序号', '创建日期', '更新日期']
    columns = [i for i in columns2 if i not in removeList]
    3、sql语句添加数据库筛选原因：
    # 获取数据库字段（原理获取服务器系统表数据【information_schema.COLUMNS】）
    # 如果服务器上不同数据库中有相同表名，会出现错误，必须通过【table_schema】对数据库筛选
    """
    sql = """
    select COLUMN_NAME from information_schema.COLUMNS
    where table_name = %s AND table_schema = %s;
    """
    params = (table, database)
    # 数据表字段获取
    return [i["COLUMN_NAME"] for i in mysql.fetchall(sql, params)]
```





# 二、笔记

## 模块安装

pip install pymysql

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

# 二、笔记

参考网站：

1. https://blog.csdn.net/giggle544/article/details/97115278

## 模块安装

pip install pymysql

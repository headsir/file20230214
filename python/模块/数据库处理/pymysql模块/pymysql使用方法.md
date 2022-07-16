## 一、pymysql封装

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
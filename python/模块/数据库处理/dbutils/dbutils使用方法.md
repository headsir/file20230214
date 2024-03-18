安装：

```
pip install dbutils
```

使用方法封装：

第一版：

DBPool.py

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @File    : DBPool.py
# @Time    : 2024/3/11 15:58
# @Author  : 978345836@qq.com
# @Software: win11 python3.9
# @Version : 1.0
# @Describe: 数据库连接池的封装

"""
程序说明：
    功能：数据库连接池的封装
"""

from loguru import logger

__all__ = ['DBPool']


# 数据库连接池 参数处理
class _BaseDBPool(object):
    """ 数据库连接池 参数处理"""

    def __init__(self, db_info: dict):
        """
        参数包含以下内容：
            - "CREATOR": "pymysql",  # 使用连接数据库的模块
            - "HOST": '127.0.0.1',  # 主机名或 IP 地址
            - "PORT": "3306",  # 端口号
            - "USER": 'root',  #  MySQL 用户名
            - "PASSWORD": 'qazwsx',  # MySQL 密码
            - "DATABASE": 'temp',  # 数据库名称
            - "CHARSET": 'utf8mb4'  #
        """
        self.dbinfo = {}
        for key, value in db_info.items():
            if key == "CREATOR" and value == "pymysql":
                from pymysql.cursors import DictCursor
                import pymysql
                value = pymysql
                # cursorclass = DictCursor
                # 统一将查询返回的数据格式转为 列表嵌套字典 [{},{}...]
                self.dbinfo["cursorclass"] = DictCursor
            if key == "PORT":
                value = int(value)
            self.dbinfo[key.lower()] = value


# 数据库连接池
class DBPool(_BaseDBPool):
    """ 数据库连接池 """

    def __init__(self, db_info: dict, maxconnections=4, mincached=2, blocking=True, ping=0):
        """
        :param db_info: 数据库相关信息的字典,**将字典参数转为关键字参数对
        :param maxconnections: 连接池允许的最大连接数，0和None 表示不限制连接数
        :param mincached: 初始化时，连接池中至少创建的连接数，0表示不创建
        :param blocking: 连接池中如果没有可用的连接后，是否阻塞等待，True，等待；False，不等待然后报错
        :param ping: ping MySQL服务器，检查是否可用。如：0、None 不检查;1、default 发请求时ping;2 创建created时ping；4 执行sql语句时ping；7 每个环节ping。

        """
        from dbutils.pooled_db import PooledDB
        import threading

        super(DBPool, self).__init__(db_info)
        self.pool = PooledDB(
            **self.dbinfo,
            maxconnections=maxconnections,
            mincached=mincached,
            blocking=blocking,
            ping=ping,
        )
        logger.debug("连接数据库为：mysql://{0}:******@{1}:{2}/{3}".format(
            self.dbinfo["user"], self.dbinfo["host"], self.dbinfo["port"], self.dbinfo["database"]))

        self.local = threading.local()

    def open(self):
        conn = self.pool.connection()
        cursor = conn.cursor()
        logger.debug("创建数据库连接")
        return conn, cursor

    def close(self, conn, cursor):
        cursor.close()
        conn.close()
        logger.debug("关闭数据库连接")

    """
    __enter__ ，__exit__ 支持with上下文管理
    storage = {
        1111: {'stack': [(conn, cursor), ]}
    }

    with 语法的使用方法：
    with db as c1:
        c1.execute("select * from")
        rs = c1.fetchall()
        with db as c2:
            c2.execute("select * from")
            rs = c2.fetchall()
        print(123)
    """

    # # 进入执行
    def __enter__(self):
        conn, cursor = self.open()
        # 给self.local对象 stack = None
        # getattr() 函数用于返回一个对象属性值。
        rv = getattr(self.local, 'stack', None)
        if not rv:
            self.local.stack = [(conn, cursor), ]
        else:
            rv.append((conn, cursor))
            self.local.stack = rv
        return conn, cursor

    # 退出执行
    def __exit__(self, exc_type, exc_val, exc_tb):
        rv = getattr(self.local, 'stack', None)
        if not rv:
            # del self.local.stack
            return
        conn, cursor = self.local.stack.pop()
        self.close(conn, cursor)
```


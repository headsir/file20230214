.ini配置文件使用

配置文件读取

```
import configparser
# 方法一：
with open(configDir, encoding="utf8") as f:
	w = f.read()
configparser.ConfigParser().read_string(w)
# 方法二：
configparser.ConfigParser().read("Config.ini", encoding="utf-8")
```

配置文件设置

```
# config.add_section("redis")  # 添加section节点
# config.set("redis", "name", "redis_admin")  # 设置指定section 的options
#
# config.remove_section("redis")  # 整个section下的所有内容都将删除
# config.remove_option("mysql", 'time')  # 删除section下的指定options
#
# config.write(open("Config", "w"))  # 保存config
```



封装

```
# -*- coding: utf-8 -*-
"""
更新日期：202301212
"""

import os
import configparser
from chardet import detect


class GetConfig:
    def __init__(self, config_file):
        """
        初始化配置文件【.ini】
        :param config_file: 配置文件路径
        """
        f = open(config_file, 'rb')
        encode = detect(f.read(10))['encoding']
        if encode not in ["utf-8"]:
            raise Exception("【%s】文件编码【%s】不可读" % (config_file, encode))
        # 实例化配置文件对象
        self.cf = configparser.ConfigParser()
        # 读取配置文件
        if os.path.isfile(config_file):  # 判断文件是否存在
            self.cf.read(config_file, encoding="utf8")
        else:  # 不存在报错
            raise FileNotFoundError

    def getSectionValue(self, section):
        """
        获取某一节点的所有配置
        :param section: 节点名
        :return: 节点配置，类型：字典
        """
        # if section in self.cf.sections():  # 判断节点是否存在，cf.sections 节点列表

        # 指示配置中是否存在指定的节
        if self.cf.has_section(section):  # 判断节点是否存在
            return dict(self.cf.items(section))
        else:
            raise Exception("【{}】不存在".format(section))

    def getOptionsValue(self, section, options):
        """
        获取某一节点下某一项配置
        :param section: 节点名
        :param options: 配置项名
        :return: 某一项配置，类型：字符串
        """
        # 指示配置中是否存在指定的节
        if self.cf.has_section(section):
            # 检查给定部分中给定选项是否存在
            if self.cf.has_option(section, options):  # 判断节点是否存在
                return self.cf.get(section, options)
            else:
                raise Exception("【{0}】不存在".format(options))
        else:
            raise Exception("【{0}】不存在".format(section))


# 配置文件地址
configDir = r"./v20220716/config.ini"
test = ReadConfig(configDir)
url = test.getOptionsValue("mysql", "username1")
# url = test.getSectionValue("mysql")
print(url)

# config = configparser.ConfigParser()
# config.read("Config.ini", encoding="utf-8")
#
# config.sections()  # 获取section节点

# config.options('mysql')  # 获取指定section 的options即该节点的所有键
#
# config.get("mysql", "name")  # 获取指定section下的options
# config.getint("mysql", "proxy")  # 将获取到值转换为int型
# config.getboolean("mysql", "pool")  # 将获取到值转换为bool型
# config.getfloat("mysql", "time")  # 将获取到值转换为浮点型
#
# config.items("mysql")  # 获取section的所用配置信息
#
# config.set("mysql", "name", "root")  # 修改db_port的值为69
#
# config.has_section("mysql")  # 是否存在该section
# config.has_option("mysql", "password")  # 是否存在该option
#
# config.add_section("redis")  # 添加section节点
# config.set("redis", "name", "redis_admin")  # 设置指定section 的options
#
# config.remove_section("redis")  # 整个section下的所有内容都将删除
# config.remove_option("mysql", 'time')  # 删除section下的指定options
#
# config.write(open("Config", "w"))  # 保存config
# # config.items()返回list，元素为tuple,转换成字典类型
# pic_path = dict(config.items("pic_path"))
# pic_path['url']
# pic_path['path']

```


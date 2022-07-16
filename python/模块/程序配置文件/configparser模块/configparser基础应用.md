.ini配置文件使用

```
# -*- coding: utf-8 -*-


import configparser
import os


class ReadConfig():
    def __init__(self, configDir):

        self.cf = configparser.ConfigParser()

        if os.path.isfile(configDir):  # 判断文件是否存在
            self.cf.read(configDir, encoding="utf8")
        else:  # 不存在报错
            raise FileNotFoundError

    def getSectionValue(self, section):
        # if section in self.cf.sections():  # 判断节点是否存在
        if self.cf.has_section(section):  # 判断节点是否存在
            return dict(self.cf.items(section))
        else:
            print("【{}】不存在".format(section))
            return "【{}】不存在".format(section)

    def getOptionsValue(self, section, options):
        if self.cf.has_section(section):
            if self.cf.has_option(section, options):  # 判断节点是否存在
                return self.cf.get(section, options)
            else:
                print("【{0}】不存在".format(options))
                return "【{0}】不存在".format(options)
        else:
            print("【{0}】不存在".format(section))
            return "【{0}】不存在".format(section)

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


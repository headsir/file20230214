# **Python标准模块之subprocess**

> 子进程管理

## 模块许可：

```
subprocess -具有可访问I/O流的子进程
有关此模块的更多信息，请参阅PEP 324。
版权所有(c) 2003-2005由彼得·阿斯特兰德<astrand@lysator.liu.se>
根据贡献者协议授权给PSF。
请参阅http://www.python.org/2.4/license了解许可详细信息。
```

## 模块介绍：

```
具有可访问I/O流的子进程
这个模块允许你生成进程，连接到它们
输入/输出/错误管道，并获取其返回代码。
有关此模块的完整描述，请参阅Python文档。

主要API
=============================================================================================
run(...): 运行一个命令，等待它完成，然后返回CompletedProcess实例。
Popen(...): 在新进程中灵活执行命令的类

常量
=============================================================================================
DEVNULL: 表示应该使用os.devnull的特殊值
PIPE:    指示应该创建管道的特殊值
STDOUT:  指示标准错误应该转到标准输出的特殊值

老的API
===============================================================================================
call(...): 运行命令，等待它完成，然后返回返回代码。
check_call(...): 与call()相同，但如果返回码不为0则引发CalledProcessError()
check_output(...): 与check_call()相同，但返回stdout的内容而不是返回代码
getoutput(...): 在shell中运行命令，等待命令完成，然后返回输出
getstatusoutput(...): 在shell中运行命令，等待它完成，然后返回一个(exitcode, output)元组
```

## 模块使用的部分包：

```python
import builtins  # 提供对Python的所有“内置”标识符的直接访问
import errno  # 处理操作系统相关的错误码
import io  # 用于读写各种类型的数据
import os  # 操作系统接口模块
import time  # 与时间相关的函数
import signal  # 提供了在 Python 中使用信号处理程序的机制
import sys  # 系统相关的参数和函数
import threading  # 基于线程的并行
import warnings  # 警告信息的控制
import contextlib  #  为 with语句上下文提供的工具
from time import monotonic as _time  # 返回一个单调时钟的值，即不能倒退的时钟
import types  # 动态类型创建和内置类型名称


try:
    import pwd  # 访问 Unix 用户账户名及密码数据库
except ImportError:
    pwd = None
try:
    import grp  # 对Unix组数据库的访问
except ImportError:
    grp = None
```

## `__all__`介绍：

> __all__ 是针对模块公开接口的一种约定，以提供了”白名单“的形式暴露接口。如果定义了__all__，其他文件中使用from xxx import *导入该文件时，只会导入 __all__ 列出的成员，可以其他成员都被排除在外。

```python
__all__ = ["Popen", "PIPE", "STDOUT", "call", "check_call", "getstatusoutput",
           "getoutput", "check_output", "run", "CalledProcessError", "DEVNULL",
           "SubprocessError", "TimeoutExpired", "CompletedProcess"]
            # 注意:我们有意排除了list2cmdline
            # 被认为是内部实现细节。issue10838。
```

使用案例：启动电脑exe软件

```python
import subprocess

# 调用exe程序
process = subprocess.Popen(["D:/Program Files/同花顺软件/同花顺/xiadan.exe"],
                           stdout=subprocess.PIPE,
                           stderr=subprocess.PIPE)

# 获取程序的标准输出和标准错误输出
stdout, stderr = process.communicate()

# 打印输出
print(stdout.decode("utf-8"))
print(stderr.decode("utf-8"))
```




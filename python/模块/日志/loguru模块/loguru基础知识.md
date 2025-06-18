**loguru日志框架**

# 前言

日志级别(严重程度递增排序)

| 级别     | 介绍                                                         |
| -------- | ------------------------------------------------------------ |
| DEBUG    | 详细信息，一般只在调试问题时使用                             |
| INFO     | 证明事情按预期工作                                           |
| WARNING  | 某些没有预料到的事情的提示，或者在将来可能会出现的问题提示，例如:磁盘空间不足，但是软件还是会照常运行 |
| ERROR    | 由于更严重的问题，软件已不能执行一些功能了                   |
| CRITICAL | 严重错误，表明软件已不能继续运行                             |

# 一、模块安装

```
pip install loguru
```

# 二、日志输出

```
logger.debug('this is a debug message')
```

还可以打印 warning、info、error、critical、success 等级别

# 三、日志文件

```
logger.add('hello.log')
```

## 3.1 指定文件中日志输出的格式、级别

```
# leve 日志级别 （低于级别日志不会记录），message 日志信息，file 文件名，line 行号
logger.add('world.log', rotation="200 MB",
            format=" {time} | {level} | {file} | {line} | {message}", level="INFO")

logger.debug('i am debug message')
logger.info('i am info message')

# 日志文件名称加信息
logger.add('hello_{time}.log')
logger.add('hello_{time:YYYY-MM-DD}.log')

# 停止写入文件
logger.remove(id)
```

## 3.2 滚动记录日志文件

```
# 超过200M就新生成一个文件
logger.add("size.log", rotation="200 MB")
# 每天中午12点生成一个新文件
logger.add("time.log", rotation="12:00")
# 一周生成一个新文件
logger.add("size.log", rotation="1 week")
```

## 3.3 指定日志文件的有效期

```
logger.add("file.log", retention="30 days") 
```

日志文件最多保留30天，30天之前的日志文件就会被清理掉

## 3.4 配置压缩文件

```
logger.add("file.log", compression="zip") 
```

# 四、异常捕获

## 4.1 catch 装饰器捕获异常

```
@logger.catch
def a_function(x):
    return 1 / x

a_function(0)
```

## 4.2 exception 方法捕获异常

```
def b_function1(x):
    try:
        return 1 / x
    except ZeroDivisionError:
        logger.exception("exception!!!")

b_function1(0)
```



# 日志封装

```python
import os
import time
from loguru import logger
// """项目根目录"""
// BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
from application.settings import BASE_DIR

"""
# 日志简单配置
# 具体其他配置 可自行参考 https://github.com/Delgan/loguru
"""

# 移除控制台输出
logger.remove(handler_id=None)

log_path = os.path.join(BASE_DIR, 'logs')
if not os.path.exists(log_path):
    os.mkdir(log_path)

log_path_info = os.path.join(log_path, f'info_{time.strftime("%Y-%m-%d")}.log')
log_path_error = os.path.join(log_path, f'error_{time.strftime("%Y-%m-%d")}.log')

info = logger.add(log_path_info, rotation="00:00", retention="3 days", enqueue=True, encoding="UTF-8", level="INFO")
error = logger.add(log_path_error, rotation="00:00", retention="3 days", enqueue=True, encoding="UTF-8", level="ERROR")
```


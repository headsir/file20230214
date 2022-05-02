python程序中使用 import XXX 时，python解析器会在当前目录、已安装和第三方模块中搜索 xxx，如果都搜索不到就会报错。

## 1. 加入上层目录和绝对路径

使用sys.path.append()方法可以临时添加搜索路径，方便更简洁的import其他包和模块。这种方法导入的路径会在python程序退出后失效。

```python
import sys
sys.path.append('..')  # 表示导入当前文件的上层目录到搜索路径中
sys.path.append('/home/model')  # 绝对路径
```

## 2. 加入当前目录

```python
import os
import sys
# os.getcwd()  # 用于获取当前工作目录
sys.path.append(os.getcwd())
```

## 3. 定义搜索优先顺序

定义搜索路径的优先顺序，序号从0开始，表示最大优先级，sys.path.insert() 加入的也是临时搜索路径，程序退出后失效。

```python
import sys
sys.path.insert(1, "./model")
sys.path.insert(1, "./crnn")
```
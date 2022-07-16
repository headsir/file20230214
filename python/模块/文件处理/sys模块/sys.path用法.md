python程序中使用 import XXX 时，python解析器会在当前目录、已安装和第三方模块中搜索 xxx，如果都搜索不到就会报错。

## 1. 加入上层目录和绝对路径

使用sys.path.append()方法可以临时添加搜索路径，方便更简洁的import其他包和模块。这种方法导入的路径会在python程序退出后失效。

```python
import sys
sys.path.append('..')  # 表示导入当前文件的上层目录到搜索路径中
sys.path.append(sys.path[1] + '\\neighbours_planning') # sys.path[1] 当前文件的上层目录
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

## 4.python中获取路径

（1）获取工作目录的路径

```scss
os.getcwd()
```

也就是命令行执行的目录，比如在桌面打开cmd窗口，os.getcwd()得到的就是好桌面路径

（2）获取python文件的路径

```lua
os.path.dirname(os.path.realpath(__file__))
```

也就是代码在哪个文件，返回的就是这个文件的绝对路径

（3）获取当前被python.exe执行的文件的路径

```less
sys.path[0]
```

比如test.py调用了test1.py，如果python.exe执行test.py，那么在test1.py中的sys.path[0]会给到test.py的路径
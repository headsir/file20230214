# 调试神器之PySnooper

## 一、基础-**装饰器**

### 1、函数知识

#### 1.1 在函数中定义函数

```python
def hi(name="yasoob"):
    print("now you are inside the hi() function")
 
    def greet():
        return "now you are in the greet() function"
 
    def welcome():
        return "now you are in the welcome() function"
 
    print(greet())
    print(welcome())
    print("now you are back in the hi() function")
 
hi()
#output:now you are inside the hi() function
#       now you are in the greet() function
#       now you are in the welcome() function
#       now you are back in the hi() function
 
# 上面展示了无论何时你调用hi(), greet()和welcome()将会同时被调用。
# 然后greet()和welcome()函数在hi()函数之外是不能访问的，比如：
 
greet()
#outputs: NameError: name 'greet' is not defined

```

#### 1.2 从函数中返回函数

其实并不需要在一个函数里去执行另一个函数，我们也可以将其作为输出返回出来：

```python
def hi(name="yasoob"):
    def greet():
        return "now you are in the greet() function"
 
    def welcome():
        return "now you are in the welcome() function"
 
    if name == "yasoob":
        return greet
    else:
        return welcome
 
a = hi()
print(a)
#outputs: <function greet at 0x7f2143c01500>
 
#上面清晰地展示了`a`现在指向到hi()函数中的greet()函数
#现在试试这个
 
print(a())
#outputs: now you are in the greet() function
```

当我们写下 `a = hi()`，``hi()`` 会被执行，而由于 name 参数默认是 yasoob，所以函数 greet 被返回了。如果我们把语句改为` a = hi(name = "ali")`，那么 welcome 函数将被返回。我们还可以打印出 `hi()()`，这会输出 **now you are in the greet() function**。

#### 1.3 将函数作为参数传给另一个函数

```python
def hi():
    return "hi yasoob!"
 
def doSomethingBeforeHi(func):
    print("I am doing some boring work before executing hi()")
    print(func())
 
doSomethingBeforeHi(hi)
#outputs:I am doing some boring work before executing hi()
#        hi yasoob!
```

### 2、装饰器

#### 2.1 第一个装饰器

来源：https://haokan.baidu.com/v?pd=wisenatural&vid=5142893912467101235

```python
# 我们希望在不修改原函数的情况下，来对函数进行扩展
def add(a, b):
    return a + b


def fn():
    print('我是fn函数....')


# 只需要根据现有的函数，来创建一个新的函数
def fn2():
    print('函数开始执行···')
    fn()
    print('函数执行结束·····')


# fn2()

def new_add(a, b):
    print('计算开始·······')
    r = add(a, b)
    print('计算结束·······')
    return r


# r = new_add(111, 222)
# print(r)

"""
上边的方式已经可以在不修改源代码的情况下对函数进行扩展，但是这种方式要求我们每扩展一个函数就要手动创建一个新的函数，
实在太麻烦了，为了解决这个问题，我们创建一个函数，让这个函数可以自动的帮助我们生产函数
"""


def begin_end(old):
    """
    用来对其他函数进行扩展，使其他函数可以在执行前打印开始执行，执行后打印执行结束
    :param old:要扩展的函数对象
    :return: 返回新函数
    """

    # 创建一个新函数
    def new_function(*args, **kwargs):
        print('开始执行······')
        # 调用被扩展的函数
        result = old(*args, **kwargs)
        print('执行结束·······')
        # 返回函数的执行结果
        return result

    # 返回新函数
    return new_function


def fn3(old):
    """
    用来对其他函数进行扩展，使其他函数可以在执行前打印开始执行，执行后打印执行结束
    :param old:要扩展的函数对象
    :return: 返回新函数
    """

    # 创建一个新函数

    def new_function(*args, **kwargs):
        print('fn3装饰器~~~~开始执行······')
        # 调用被扩展的函数
        result = old(*args, **kwargs)
        print('fn3装饰器~~~~执行结束·······')
        # 返回函数的执行结果
        return result

        # 返回新函数

    return new_function


f = begin_end(fn)
f2 = begin_end(add)

# r =f()
# r = f2(123,456)
# r = f3(123,456)
# print(r)

"""
想begin_end()这种函数我们就称它为装饰器，通过装饰器，可以在不修改原来函数的情况下
来对函数进行扩展，在开发中，我们都是通过装饰器来扩展函数的功能的
"""


@fn3
@begin_end
def say_hello():
    print('大家好······')


say_hello()
```

## 二、PySnooper使用

### 1、安装

​	使用pip安装

```undefined
pip install pysnooper 
```

### 2、使用

案例1：

```python
import numpy as np
import pysnooper


@pysnooper.snoop()
def one(number):
    mat = []
    while number:
        mat.append(np.random.normal(0, 1))
        number -= 1
    return mat


one(3)

# 输出

Starting var:.. number = 3
22:17:10.634566 call         6 def one(number):
22:17:10.634566 line         7     mat = []
New var:....... mat = []
22:17:10.634566 line         8     while number:
22:17:10.634566 line         9         mat.append(np.random.normal(0, 1))
Modified var:.. mat = [-0.4142847169210746]
22:17:10.634566 line        10         number -= 1
Modified var:.. number = 2
22:17:10.634566 line         8     while number:
22:17:10.634566 line         9         mat.append(np.random.normal(0, 1))
Modified var:.. mat = [-0.4142847169210746, -0.479901983375219]
22:17:10.634566 line        10         number -= 1
Modified var:.. number = 1
22:17:10.634566 line         8     while number:
22:17:10.634566 line         9         mat.append(np.random.normal(0, 1))
Modified var:.. mat = [-0.4142847169210746, -0.479901983375219, 1.0491540468063252]
22:17:10.634566 line        10         number -= 1
Modified var:.. number = 0
22:17:10.634566 line         8     while number:
22:17:10.634566 line        11     return mat
22:17:10.634566 return      11     return mat
Return value:.. [-0.4142847169210746, -0.479901983375219, 1.0491540468063252]
```

这是最简单的使用方法，从上述输出结果可以看出，`PySnooper` 输出信息主要包括以下几个部分：

- 局部变量值
- 代码片段
- 局部变量所在行号
- 返回结果

也就是说，把我们日常DEBUG所需要的主要信息都输出出来了。

上述结果输出到控制台，还可以把日志<font color=red size=5>输出到文件</font>

```python
@pysnooper.snoop("debug.log")
```

### 3、功能

#### 3.1 日志输出到文件

```
@pysnooper.snoop(output="./debug.log")
```

#### 3.2 日志添加前缀

参数<font color=gree size=5>***prefix***</font>给debug信息添加前缀的方式便于识别

```
@pysnooper.snoop(prefix="funcTwo ")
```

#### 3.3 查看非局部变量

参数<font color=gree size=5>***watch***</font>用来设置跟踪的非局部变量

```
@pysnooper.snoop(watch=("test.t", "x"))
```

#### 3.4 展开字典或列表

参数<font color=gree size=5>***watch_explode***</font>表示设置的变量都不监控，只监控没设置的变量

```
@pysnooper.snoop(watch_explode="d")
```

#### 3.5 设置跟踪函数的深度

参数<font color=gree size=5>***depth***</font>参数来设置跟踪深度（不指定的话默认为 1）

```
@pysnooper.snoop(depth=2)
```

#### 3.6 设置最大的输出长度

默认情况下，PySnooper 输出的变量和异常信息，如果超过 100 个字符，被会截断为 100 个字符

参数<font color=gree size=5>***max_variable_length***</font>来设置长度，None表示从不截断

```
@pysnooper.snoop(max_variable_length=None)
```

#### 3.7 多线程调试模式

参数 <font color=gree size=5>***thread_info=True***</font>打印出是在哪个线程对变量进行的修改

```
@pysnooper.snoop(max_variable_length=thread_info=True)
```

#### 3.8 自定义对象的格式输出

参数<font color=gree size=5>***custom_repr***</font>它接收一个元组对象,你可以指定特定类型的对象以特定格式进行输出

```
@pysnooper.snoop(custom_repr=(输出的对象,自定义格式函数))
```

#### 3.9 代码运行的时间

参数<font color=gree size=5>***relative_time***</font>代码运行的时间

```
@pysnooper.snoop(relative_time=True)
```




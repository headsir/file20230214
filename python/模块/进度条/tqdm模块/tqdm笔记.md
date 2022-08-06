# tqdm

```
# encoding=utf-8
from tqdm import tqdm

"""
模块安装：pip install tqdm
tqdm 常见参数介绍:
iterable=None  #可迭代的对象，在手动更新时不需要进行设置
desc=None #字符串，左边进度条描述文字
total=None #总的项目数
leave=True #布尔值，迭代完成后是否保留进度条
file=None #输出指向位置，默认是终端，一般不需要设置
ncols=None #调整进度条宽度，默认是根据环境自动调节长度，如果设置为0，就没有进度条，只有输出的信息。
mininterval=0.1, 
maxinterval=10.0,
miniters=None,
ascii=None, 
disable=False, 
unit='it' #描述处理项目的文字，默认是‘it',例如：100 it/s，处理照片的话设置为’img'，则为100 img/s。
unit_scale=False 自动根据国际标准进行项目处理速度单位的换算，例如 100000 it/s >>100k it/s
dynamic_ncols=False, smoothing=0.3, bar_format=None, initial=0,
position=None, postfix=None, unit_divisor=1000, write_bytes=None,
lock_args=None, nrows=None, colour=None, delay=0, gui=False,**kwargs)k:
"""


def processing(iterablet, desc="自定义进度条", ncols=None, leave=True, colour='green', total=None):
    processings = tqdm(iterable=iterablet, desc=desc, ncols=ncols, leave=leave, colour=colour, total=total)

    return processings

# pat = processing(range(100000000))
# for i in pat:
#     # pat.set_description("Processing %d" % i)
#     i
#     # print(i)

```


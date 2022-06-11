sys模块包括了一组非常实用的服务，内含很多函数方法和变量，用来处理Python运行时配置以及资源，从而可以与前当程序之外的系统环境交互。

## 1、命令行参数List

​		`sys.argv`第一个元素是程序本身路径

​	[用法](https://www.cnblogs.com/liangmingshen/p/8906148.html)

## 2、返回系统导入的模块字段

​		`sys.modules ` keys是模块名，values是模块

​	[用法](https://www.cnblogs.com/zhaojingyu/p/9069076.html)

## 3、退出程序

​		`sys.exit`

​	[用法](https://www.cnblogs.com/pfeiliu/p/14248303.html)

## 4、返回模块的搜索路径

​		`sys.path` 初始化时使用PYTHONPATH环境变量的值

​	[用法](.\sys.path用法.md)

## 5、标准输出

​		`sys.stdout.write('output') `

​	[用法](https://zhuanlan.zhihu.com/p/377418978)

## 6、标准输入

​		`sys.stdin.readline()[:-1]`

## 7、获取当前正在处理的异常类

​		`exc_type`、`exc_value`、`exc_traceback`

​		当前处理的异常详细信息`sys.exc_info() `

```
#获取Python解释程序的版本值，16进制格式如：0x020403F0sys.hexversion    
#获取Python解释程序的版本sys.version     
 #解释器的C的API版本sys.api_version   
#解释器版本信息 返回major=3, minor=6, micro=6, releaselevel='final', serial=0#‘final’表示最终,也有’candidate’表示候选，serial表示版本级别，是否有后继的发行sys.version_info
#如果value非空，这个函数会把他输出到sys.stdout，并且将他保存进__builtin__._.指在python的交互式解释器里，’_’ 代表上次你输入得到的结果，hook是钩子的意思，将上次的结果钩过来sys.displayhook(value)  

#打印出给定的回溯和异常sys.stderr。sys.excepthook（type，value,callback） 
#返回当前你所用的默认的字符编码格式sys.getdefaultencoding()  
#返回将Unicode文件名转换成系统文件名的编码的名字sys.getfilesystemencoding()
 #用来设置当前默认的字符编码，如果name和任何一个可用的编码都不匹配，抛出 LookupError，这个函数只会被site模块的sitecustomize使用，一旦别site模块使用了，他会从sys模块移除sys.setdefaultencoding(name)
#Python解释器导入的模块列表sys.builtin_module_names   
#Python解释程序路径sys.executable       
#获取Windows的版本sys.getwindowsversion()    
#记录python版权相关的东西sys.copyright    
#本地字节规则的指示器，big-endian平台的值是’big’,little-endian平台的值是’little’sys.byteorder     
#用来清除当前线程所出现的当前的或最近的错误信息sys.exc_clear()   
#返回平台独立的python文件安装的位置sys.exec_prefix  
#错误输出sys.stderr       
#标准输入sys.stdin    
#标准输出sys.stdout  
#返回操作系统平台名称sys.platform    
#最大的Unicode值sys.maxunicode   
#最大的Int值sys.maxint     
#呼叫func(*args)，同时启用跟踪。跟踪状态被保存，然后恢复。#这是从调试器从检查点调用，以递归调试其他一些代码。sys.call_tracing（func，args ) 
#清除内部类型缓存。类型缓存用于加速属性和方法查找sys._clear_type_cache（） 
#返回一个字典，将每个线程的标识符映射到调用该函数时该线程中当前活动的最顶层堆栈帧。sys._current_frames（） 
#指定Python DLL句柄的整数。sys.dllhandle #如果这是真的，Python将不会尝试在源模块的导入上编写.pyc或.pyo文件。#此值最初设置为True或 False取决于-B命令行选项#和 PYTHONDONTWRITEBYTECODE 环境变量，但您可以自己设置它来控制字节码文件的生成。sys.dont_write_bytecode 
#获取检查间隔sys.getcheckinterval()
#返回用于dlopen()调用的标志的当前值。标志常量在dl和DLFCN模块中定义。可用性：Unix。sys.getdlopenflags（） 
#返回对象的引用计数。返回的计数通常比您预期的高一个，因为它包含（临时）引用作为参数getrefcount()。sys.getrefcount（对象）
#以字节为单位返回对象的大小。对象可以是任何类型的对象。所有内置对象都将返回正确的结果，但这不一定适用于第三方扩展，因为它是特定于实现的。#如果给定，则如果对象未提供检索大小的方法，则将返回default。否则TypeError将被提出。#如果对象由垃圾收集器管理，则调用该对象的方法并添加额外的垃圾收集器开销。sys.getsizeof（对象[，默认] ） 
#从调用堆栈返回一个框架对象。如果给出了可选的整数深度，#则返回堆栈顶部下方多次调用的帧对象。如果它比调用堆栈更深，#ValueError则引发。深度的默认值为零，返回调用堆栈顶部的帧。sys._getframe（[ 深度] ） 
#获取设置的探查器功能setprofile()。sys.getprofile（） 
#获取设置的跟踪功能settrace()。sys.gettrace（） 
#返回一个描述当前正在运行的Windows版本的命名元组sys.getwindowsversion（）
```
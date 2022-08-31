# Pywinauto模块

# 一、安装模块

````
pip install Pywinauto
````

# 二、确定应用程序的可访问技术

支持控件的访问技术：

1、WIN32 API (backend = “win32”) 默认的backend

​		简单的WinForms控件和大多数旧的应用程序

2、MS UI Automation API (backend = “uia”)

​	WinForms、WPF、Store apps 、Qt5、浏览器

# 三、切入点

切入点主要是限制自动化控制进程的范围。如一个程序有多个实例，自动化控制一个实例，而保证其他实例（进程）不受影响。

在pywinauto中主要有两种对象可以建立这种入口点：

- Application:

  Application的作用是一个进程，如一般的桌面应用程序都为此类。

- Desktop

  Desktop的作用可以跨进程。主要用于一个程序可以包含多个实例（进程）的程序。

# 四、检测工具

1、Inspect.exe

2、spy++.exe，它使用 Win32 API，如果 Spy++ 能够显示程序的所有控件，那么该应用程序适合使用win32的backend

3、ViewWizard(窗口信息查看精灵)

# 五、打开应用程序

1、打开windos自带的记事本：

```
from pywinauto.application import  Application
app = Application(backend="win32").start("notepad.exe")
```

2、打开任意的windos程序：

```
from pywinauto.application import  Application
app = Application(backend="后端类型").start("程序位置路径")
```

# 六、连接已经打开的应用程序

1、通过进程号连接

2、通过窗口句柄连接

```
from pywinauto.application import  Application
app = Application(backend="uia").connect(process="进程号")
app = Application(backend="uia").connect(handle="窗口句柄")
```

# 七、选择应用程序窗口

1、根据窗口标题或类名选择

dlg = app[窗口类名/标题]

2、根据窗口类名选择

dlg = app.窗口类名

==非英文程序 推荐使用方式一==

```
# 打印窗口中所有的控件
dlg.print_control_identifiers()
```

# 八、应用程序窗口操作方法

- 最大化：dlg.maxmize()
- 最小化：dlg.minmize()
- 恢复正常大小：dlg.restore()
- 查看窗口显示状态：dlg.get_show_state()  # 最大化 返回1，正常返回0
- 关闭窗口：dlg.close()
- 获取当前窗口显示的坐标：dlg.rectangle()

# 九、窗口控件的相关操作
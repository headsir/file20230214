# Pywinauto模块

官方文档：https://www.kancloud.cn/gnefnuy/pywinauto_doc

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
  
  ```
  app = Desktop(backend="uia")
  ```
  
  

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
# 打印可见控件
dlg.children()
```

3、获取应用程序所有得窗口

app.windows()

# 八、应用程序窗口操作方法

- 最大化：dlg.maxmize()
- 最小化：dlg.minmize()
- 恢复正常大小：dlg.restore()
- 查看窗口显示状态：dlg.get_show_state()  # 最大化 返回1，正常返回0
- 关闭窗口：dlg.close()
- 获取当前窗口显示的坐标：dlg.rectangle()

# 九、窗口控件的相关操作

打印窗口中所有的控件`dlg.print_control_identifiers()`

## 1、选择控件

- menu = dlg.控件名称
- menu = dlg[控件名称],==推荐方式==
- file = menu.child_window(title=“文件”)

## 2、控件的分类(部分)

| 控件类型 | 控件代码  | 控件类型       | 控件代码   | 控件类型     | 控件代码 |
| -------- | --------- | -------------- | ---------- | ------------ | -------- |
| 状态栏   | StatusBar | 静态内容       | Static     | 按钮         | Button   |
| 复选框   | CheckBox  | 单选框         | RadioButto | 组框         | GroupBOX |
| 组合框   | ComboBox  | 对话框（窗口） | Dialog     | 编辑栏       | Edit     |
| 头部内容 | Header    | 列表框         | ListBox    | 列表显示控件 | ListView |
| 弹出菜单 | PopupMenu | 选项卡控件     | TabControl | 工具栏       | Toolbar  |
| 工具提示 | ToolTips  | 树状视图       | Tree View  | 菜单         | Menu     |
| 菜单项   | MenuItem  | 窗格           | Pane       | 标题栏       | TitleBar |

## 3、控件基本属性的获取方法

- 获取控件类型：wrapper_object()
- 获取该控件支持的方法：print(dir(a. wrapper_object())) 
- 获取控件的子元素：children
- 获取控件类名：class_name
- 以字典形式返回控件的属性：get_properties

## 4、窗口及控件截图处理

方法：capture_as_image

pic = dlg.capture_as_image()

pic.save(xxx.png)

# 十、菜单控件的相关操作

## 1、菜单相关操作

菜单（Menu）的方法

items：获取所有的子菜单

item_by_index：根据索引选定指定的菜单项

item_by_path：根据路径指定菜单项，==推荐==

```
menu.item_by_path(“菜单”)
menu.item_by_path(“菜单->新建连接”)
```

## 2、菜单项相关操作

菜单项（MenuItem）的方法

items：获取所有的子选项

click_input：点击菜单

## 3、获取标签坐标点

menu.rectangle().mid_point(),返回中心点

menu.rectangle()，返回坐标

# 十一、等待机制

## 1、Wait方法

> 作用：等待窗口处于某个状态
>
> 参数：
>
> - wait_for:等待的状态（状态有以下几种）
>   - exists：表示该窗口是有效的句柄
>   - visble：表示该窗口未隐藏
>   - enabled：表示未禁用窗口
>   - ready：表示该窗口可见并启用
>   - active：表示该窗口处于活动状态
>
> - timeout：超时时间
> - retry_interval：重试时间间隔

## 2、 Wait_not方法

> 作用：等待窗口不处于某个特定状态
>
> 参数：
>
> - wait_for_not:等待的状态（状态有以下几种）
>   - exists：表示该窗口是有效的句柄
>   - visble：表示该窗口未隐藏
>   - enabled：表示未禁用窗口
>   - ready：表示该窗口可见并启用
>   - active：表示该窗口处于活动状态
>
> - timeout：超时时间
> - retry_interval：重试时间间隔

## 3、wait_cpu_usage_lower方法

> 等待该进程的cpu的使用率低于某个阈值
>
> 注意：此方法仅适用于整个应用程序进程，不适用于窗口/元素
>
> 参数：
>
> threshold：该进程cpu占用率
>
> timeout：超时时间
>
> retry_interval：重试时间间隔

## 4、timings模块

- wait_until方法：

  参数：

  - Timeout：超时时间
  - retry_interval 重试时间
  - func 执行的函数
  - value 比较的值
  - Op 比较方式函数（默认为相等）
  - args 给执行函数传位置参数
  - kwargs 给执行函数传关键字参数

- 全局计时变量值的设置方法
  - Timings.defaults()：将全局计时设为默认值
  - Timings.slow()：将所有时间加倍（使脚本执行速度降低约2倍）
  - Timings.fast()：将所有计时除以2（快2倍）

# 十二、编辑类控件(Edit)的基本操作

```
import pywinauto

app = pywinauto.Application().start("notepad.exe")  # 记事本 backend="win32"
# 选择主窗口
dlg = app["无标题 - 记事本"]
# 打印主窗口控件
# dlg.print_control_identifiers()
# 选择编辑器
dlg["Edit"].type_keys("python做自动化非常方便，666")
#重新连接窗口
# process_id = dlg.process_id()
#
# app_menu = pywinauto.application.Application(backend="uia").connect(process=process_id)
# app_dlg=app_menu["无标题 - 记事本"]


# 替换操作
# 通过菜单选择替换
dlg.menu_select("编辑->替换(R)")  # 记事本特有

# 选择替换窗口
app["替换"].print_control_identifiers()
# 选择替换编辑框
app["替换"]["Edit1"].type_keys("666")
# 选择替换为编辑框
app["替换"]["Edit2"].type_keys("999")

# 选择全部替换按钮，进行点击
app["替换"]["Button3"].click()
```

# 十三、模拟键盘操作

## 1、操作模块：keyboard

Send_keys方法：

> 使用方式：send_keys(按键)
>
> 例如：
>
> 按F5：send_keys(“{VK_F5}”)
>
> 按F12：send_keys(“{VK_F12}”)
>
> 按回车键：send_keys(“{VK_RETURN}”) 
>
> 按普通字母键：send_keys(‘A’)

常用按键参考

| 键名        | 代码           | 键名    | 代码          | 键名   | 代码           |
| ----------- | -------------- | ------- | ------------- | ------ | -------------- |
| ESC键       | VK_ESCAPE(27)  | 回车键  | VK_RETURN(13) | TAB键  | VK_TAB(9)      |
| Caps Lock键 | VK_CAPITAL(20) | Shift键 | VK_SHIFT(16)  | Ctrl键 | VK_CONTROL(17) |
| Alt键       | VK_MENU(18)    | 空格键  | VK_SPACE(32)  | 退格键 | VK_BACK(8)     |
| 左win键     | VK_LWIN(91)    | 右win键 | VK_RWIN(921)  |        |                |

## 2、键盘修饰符的使用

| 功能    | 修饰符 | 功能   | 修饰符 | 功能  | 修饰符 | 功能                   | 修饰符 |
| ------- | ------ | ------ | ------ | ----- | ------ | ---------------------- | ------ |
| 按Shift | “+”    | 按Ctrl | “^”    | 按Alt | “%”    | 按Ctrl+S进行保存的操作 | “^s”   |

# 十四、模拟用户鼠标操作

函数：mouse

> 鼠标单击：click
>
> - 参数：
>   - 默认左键点击
>   - coords,传入点击位置
>
> 鼠标双击：doubl_click
>
> - 参数：
>   - button，传入左右，默认”left“
>   - coords,传入点击位置
>
> 鼠标右击：right_click
>
> 鼠标中间点击：wheel_click
>
> 按下鼠标：press
>
> 释放鼠标：repleace
>
> 鼠标移动：move
>
> 滚动鼠标：scroll
>
> - 参数：
>   - wheel_dist，传入滚动单位，正往上，负往下
>   - coords,传入点击位置

```
实例：
mouse.click(button='left', coords=(cords.left + 10, cords.top + 10))
```



# 十五、访问系统通知区域

方式一：通过Explorer

在时钟附近有表示正在运行的应用程序的图标，该区域通常被称为”系统托盘“，也称为通知区域。该区域的访问，可以通过启动”Explorer.exe“这个应用程序，可以在‘任务栏’这个窗口中找到有标题为‘用户提示通知区域’的工具栏控件。

```
app = Application(backend="uia").connect(path="explorer")
# 底部状态栏
icons = app['任务栏']['用户提示通知区域']
```

## 隐藏通知区域的窗口检测和操作

```
task = app['任务栏'].child_window(title="通知 V 形", auto_id="1502", control_type="Button")
task.click()
# 选择通知溢出的窗口，然后进行相关的操作
app["通知溢出"].print_control_identifiers()
app["通知溢出"]["小智桌面"].click()
```

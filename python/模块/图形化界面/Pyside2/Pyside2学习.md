# # 一、Pyside2模块安装

```cmd
pip install Pyside2
```

版本：PySide2  5.15.2.1

# 二、配置Pycharm外部工具

Qt Designer:可视化编辑UI界面，生成.ui文件。
PySide-uic: 将.ui文件转换为.py文件，以便修改和导入。
PySide-rcc: 将.qrc文件（资源文件）转换为.py文件。
PySide-lupdate:用于从ui文件和Python源码中提取需要翻译的字符串
linguist:用于多语言翻译，输入文件为PySide-lupdate生成的`*.ts`文件，并可以发布为`*.qm`文件

工具配置建议：

```python
Qt Designer:
    名称：任意即可（建议QtDesigner）
    程序：程序所在的路径
    参数：$FileNameWithoutExtension$.ui(可选参数)
    工作目录：$ProjectFileDir$

pyside2-rcc:
    名称：任意即可（建议qrc2py,这样一目了然）
    程序：程序所在目录
    参数：$FileName$ -o $FileNameWithoutExtension$_rc.py
    工作目录：$FileDir$

pyside2-uic:
    名称：任意即可（建议ui2py,理由同上）
    程序：程序所在目录
    参数：-o $FileNameWithoutExtension$_ui.py $FileName$
    工作目录：$FileDir$
    
PySide-lupdate: 
    名称：任意即可
    程序：程序所在目录
    参数：-verbose $FileNameWithoutExtension$.pro
    工作目录:$FileDir$

linguist:   
    名称：任意即可
    程序：程序所在目录
    参数：$FileNameWithoutExtension$.ts
    工作目录:$FileDir$
```

# 三、快速入门

来源：https://www.zhihu.com/people/si-tu-32-83/posts

## 快速入门一：

==主要是简单地介绍一下PySide，并使用designer制作了一个演示用的小demo程序==

​		PySide（在本文中代指PySide2和PySide6）是一个Python的图形化界面（GUI）库，由C++版的Qt开发而来，在用法上基本与C++版没有特别大的差异。相对于其他Python  GUI库来说，PySide开发较快，功能更完善，而且文档支持更好。我这里就给大家简单写一个极简的快速入门教程，旨在帮助大家快速上手并使用这个库。

​		首先说一下PySide和PyQt（在本文中代指PyQt5和PyQt6）的关系，前者是Qt公司的产品，后者是第三方公司的产品，二者用法基本相同，不过在使用协议上却有很大差别，PySide可以在LGPL协议下使用，PyQt则在GPL协议下使用，这两个协议的区别，大家可以自行搜索，为了避免潜在的问题，我这里推荐使用PySide。

​		现在说一下PySide2和PySide6的区别，也就是PyQt5和PyQt6的区别。PySide2和PyQt5由C++版的Qt5开发而来.，而PySide6和PyQt6对应的则是C++版的Qt6。从PySide6开始，PySide的命名也会与Qt的大版本号保持一致，不会再出现类似PySide2对应Qt5这种容易混淆的情况。

​		在使用层面上，PySide2/PyQt5和PySide6/PyQt6并无过多的差异，只有一点需要注意，使用PySide6/PyQt6开发的程序在默认情况下，不兼容Windows7系统，这也是Qt6所决定的。在下面和之后的文章中，我会尽量兼容PySide2和PySide6这两个版本，如果有存在差异的地方，我会两个版本分别解释。

​		那么开始进入正题，先介绍一下使用PySide开发GUI的基本知识（PyQt同理，下面就不再提它了）。

​		PySide为我们提供了两种开发界面的方式，一种叫QtWidget，是在网上搜到的教程中最常见的方式；另一种叫QML，是一种新型的开发方式，也是Qt正在努力推广的开发方式。在本系列的文章中，我们主要使用QtWidget这种方式，而使用QtWidget开发程序时，也有两种基本的使用方法，一种是通过designer开发界面，另一种是用过代码手动开发界面，这里我们的目的是极简快速入门，所以使用designer这种方便的方式进行开发。

### designer介绍

designer界面如下：

![image-20231219112603776](imge/Pyside2学习.assets/image-20231219112603776.png)

组件选择区：用来选择组件，鼠标选择组件后就可以拖拽到窗口界面上，在本教程中会使用到一部分这里的组件。

工作区：用来布置界面，调整窗口，我们可以把工作区中的窗口随意拖拽、调整大小；

对象查看区：查看界面上共有多少组件，以及它们的布局关系；

属性设置区：设置组件的属性，如文本、大小、名称等。

这里我们先把工作区中的窗口拖放到中间，再从组件选择区里拖拽一个Label组件到窗口上，如下图所示：

![image-20231222174946152](imge/Pyside2学习.assets/image-20231222174946152.png "拖放组件")

​		之后我们双击窗口里面的Label，输入“你好啊！PySide2~”，我们会发现默认的Label大小并不能完全展示文本，所以我们把Label的宽度拉长（单击Label后，拖住周围的深蓝色方块即可调整大小）：

![image-20231222180332898](imge/Pyside2学习.assets/image-20231222180332898.png "输入文字")

接下来我们保存这个界面（Ctrl+S），或者在菜单栏选择“文件”->“保存”，我们命名为hello.ui。

注意，PySide2是不能直接使用ui文件的，我们还需要将其转为py文件，这一步我们使用命令：

pyside2-uic hello.ui > hello_ui.py  (还可使用pyside2-uic工具，[工具配置：](# 二、配置Pycharm外部工具  "外部工具配置"))

​		这样我们就会得到一个hello_ui.py文件，我们可以打开看一下，里面都是一些界面代码，如果不使用designer，那么这个hello_ui.py就需要我们手动来写，所以，使用designer可以加快开发速度，减少工作量，提升开发效率。

接下来我们就要写代码来把界面展示出来，新建一个py文件：

```python
# -*- coding: utf-8 -*-
# 导入sys
import sys
# 任何一个PySide界面程序都需要使用QApplication
# 我们要展示一个普通的窗口，所以需要导入QWidget，用来让我们自己的类继承
from PySide2.QtWidgets import QApplication, QWidget


# 继承QWidget类，以获取其属性和方法
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        # 加在UI文件2种方式：静态加载、动态加载
        # 导入我们生成的界面【静态加载ui.py文件】
        from hello_ui import Ui_Form
        # 设置界面为我们生成的界面
        # self.ui = Ui_Form()
        # self.ui.setupUi(self)

        # 导入我们生成的界面【动态加载ui文件】
        from PySide2.QtUiTools import QUiLoader
        self.ui = QUiLoader().load("hello.ui", self)


# 程序入口
if __name__ == "__main__":
    # 初始化QApplication，界面展示要包含在QApplication初始化之后，结束之前
    app = QApplication(sys.argv)

    # 初始化并展示我们的界面组件
    window = MyWidget()
    window.show()

    # 结束QApplication
    sys.exit(app.exec_())
    # 注意，在PySide6中，需要使用app.exec()
    # sys.exit(app.exec())
```

注释中包含了详细的代码说明，接下来我们只要执行这个py文件，就可以看到我们的界面了：

![image-20231226110549382](imge/Pyside2学习.assets/image-20231226110549382.png)

## 快速入门二：

### 界面布局

​		学习如何让组件在界面上排列得更有顺序，也就是如何使用布局。

​		在日常的开发中，我们经常会用到三种基础布局：水平布局、垂直布局和栅格布局（也可以叫网格布局），接下来我们来一个个地学习它们。

​		首先来看一个不使用布局的例子，我们打开designer，创建一个Widget，并在界面上摆放三个PushButton，分别双击它们，将它们的显示文字改为按钮1、按钮2和按钮3，如下图所示：

![image-20231226111143888](imge/Pyside2学习.assets/image-20231226111143888.png "三个按钮")

​		接下来我们点击designer菜单栏中的窗体->预览，或使用快捷键ctrl+r，打开界面预览，这时我们可以看到，和我们设计好的一样，三个按钮基本是在界面中间的：

![image-20231226111220249](imge/Pyside2学习.assets/image-20231226111220249.png "界面预览")



​		现在我们点击界面右上角的最大化按钮，让界面全屏，会发现，三个按钮的位置发生了变化，它们并没有继续留在界面中间，而是跑去了左上角：

![image-20231226111403823](imge/Pyside2学习.assets/image-20231226111403823.png "界面出问题")

这显然和我们预期的结果不一样，我们需要组件随着界面一起发生变化，来保证整个界面的一致性，这个时候，就需要来使用布局了。

### 水平布局

​		我们先来了解水平布局，顾名思义，水平布局就是将所有组件按照水平方向排列，实现起来也很简单，我们在界面的空白处点击鼠标右键，选择布局->水平布局即可。

![image-20231226111738502](imge/Pyside2学习.assets/image-20231226111738502.png "实现布局")

实现的效果如下：

![image-20231226111821959](imge/Pyside2学习.assets/image-20231226111821959.png "水平布局")

我们可以任意拖动这三个按钮，来改变它们的排列顺序，但是无论怎么拖动，它们都会保持水平排列。

这个时候我们再预览，放大，就会发现之前的问题已经得到了解决，无论我们怎样改变界面大小，按钮的位置都不会发生改变。

### 垂直布局

垂直布局和水平布局类似，我们只需要在选择布局时选择垂直布局，三个按钮就会按照垂直方向排列，如下图所示：

![image-20231226112005223](imge/Pyside2学习.assets/image-20231226112005223.png "垂直布局")

同样，我们也可以拖动按钮来改变它们的排列顺序。

### 栅格布局

​		接下来主要说一下栅格布局，栅格布局和水平/垂直布局不同，它首先将整个界面按照行和列进行划分，形成一个个单元格区域，之后判断哪个组件放在哪些区域，来实现一种稍微复杂的布局。

我们首先打破之前实现的垂直布局，方式为选择布局->打破布局，并重新把三个按钮摆放成如下的形式：

![image-20231226112151067](imge/Pyside2学习.assets/image-20231226112151067.png "准备使用栅格布局")

之后我们实现栅格布局，方式为布局->栅格布局，发现界面变成了这样：

![image-20231226112248438](imge/Pyside2学习.assets/image-20231226112248438.png "栅格布局")

不难看出，designer将整个界面划分为了两行两列，共四个单元格区域，三个按钮分别占了一个区域：

![image-20231226112357387](imge/Pyside2学习.assets/image-20231226112357387.png "栅格布局区域划分")

​		这里将界面划分为几行几列，以及每个组件占有的区域个数，是由designer根据我们排列组件的相对大小和相对位置自动决定的，如果我们把按钮3的宽度大致调整为按钮1和按钮2的宽度之和，如下图所示：

![image-20231226112520229](imge/Pyside2学习.assets/image-20231226112520229.png "准备使用栅格布局2")

那么在生成的栅格布局中，按钮3就会占有2个区域：

![image-20231226112624120](imge/Pyside2学习.assets/image-20231226112624120.png "栅格布局2")

​		以上就是三种基础布局的用法，在实际的开发中，最常用到的布局方式是以上三种布局的组合，比如，我们要实现一个简单的登录界面，需要使用水平布局来排列用户名和密码的文本标签和输入框，之后使用垂直布局来实现它们的上下排列，如果我们还需要放置一个登陆说明或图片，可能还需要用到栅格布局。

### 布局案例

​		我们可以使用Frame来实现上述界面，首先让组件在Frame中布局，之后再对不同的Frame进行布局。我们先把3个按钮删掉（不要忘记打破布局），然后拖放4个Frame到界面上（注意，Frame是近似透明的，如果忘记了它们被放在了哪里，就在对象查看区选中它们，就能在界面上显示了）：

​		之后我们分别拖放一个Lable和LineEdit到一个Frame中，将Label的显示文本改为用户名并对Frame应用水平布局（在Frame中点击鼠标右键）：

![image-20231226113215156](imge/Pyside2学习.assets/image-20231226113215156.png "在Frame中布局")

​		同理，我们再创造一个密码Frame和一个只含有登录按钮的Frame，然后再创建一个含有Label说明的Frame，并将它们按如下方式摆放：

![image-20231226113713256](imge/Pyside2学习.assets/image-20231226113713256.png "准备将布局结合使用")

然后应用栅格布局：

![image-20231226115129411](imge/Pyside2学习.assets/image-20231226115129411.png "简陋的登录界面")

一个还算凑合的登录界面就实现了。

​		我们主要学习了怎样在designer中使用PySide提供的三种基础布局。在任何语言中，布局都是界面开发的基础，任何精美的界面也离不开良好的布局。

## 快速入门三:

### 基础窗口-主窗口（QMainWindow）

官方文档：

- PySide2: https://doc.qt.io/qtforpython-5/PySide2/QtWidgets/QMainWindow.html
- PySide6: https://doc.qt.io/qtforpython/PySide6/QtWidgets/QMainWindow.html

​		QMainWindow具有很多的函数（方法），这里介绍比较常见的用法。

​		首先需要知道，QMainWindow是一种特殊的QtWidget，只是在普通的QtWidget上增加了一些更方便的功能，经常被用作一个程序的主界面。

学习如何使用QMainWindow，和往常一样，我们打开designer，选择创建一个Main Window（注意不是Widget）：

![image-20231226120000625](imge/Pyside2学习.assets/image-20231226120000625.png "一个MainWindow")

​		我们可以在designer右上角的对象查看区看到，默认生成的QMainWindow包含三个部分-centralwidget（中央组件）、menubar（菜单栏），statusbar（状态栏），分别对应界面中如下的几个部分：

![image-20231226120221922](imge/Pyside2学习.assets/image-20231226120221922.png "MainWindow的简单组成")

​		首先我们来创建菜单，双击左上角的“在这里输入”，输入“菜单1”并回车，就可以创建一个菜单，但是目前菜单中还没有选项，所以我们还需要添加菜单的选项（在PySide中叫做动作，action），这里需要注意，designer不支持给action直接输入中文，所以我们要先输入英文字母，再到属性设置区更改为中文。接下来我们新建一个选项1，单击“菜单1”，就会出现下拉菜单，双击“在这里输入”并回车：

![image-20231226124608115](imge/Pyside2学习.assets/image-20231226124608115.png "更改选项属性")

​		接下来我们再使用相同的办法，创建一个动作2选项，将它的对象名称改为action2。这样我们的一个菜单就建好了，按照相同的步骤我们可以再建立菜单2、菜单3……

然后我们放置一个Label到中央组件上：

![image-20231226124821040](imge/Pyside2学习.assets/image-20231226124821040.png "放置个Label")

### 案例		

​		现在，我们准备实现这样一个程序，单击动作1或动作2，更改Label显示不同的文本，同时状态栏记录我们当前的鼠标正在点击哪个选项。

我们先保存这个界面，命名为main.ui，使用pyside2-uic命令生成ui_main.py，之后创建如下的python文件（main.py）：

```python
# -*- coding: utf-8 -*-
import sys
# 因为我们创建的界面是MainWindow，所以这里要继承QMainWindow
from PySide2.QtWidgets import QApplication, QMainWindow
from PySide2 import QtGui, QtCore, QtUiTools


class UiLoader(QtUiTools.QUiLoader):
    _baseinstance = None

    def createWidget(self, classname, parent=None, name=''):
        if parent is None and self._baseinstance is not None:
            widget = self._baseinstance
        else:
            widget = super(UiLoader, self).createWidget(classname, parent, name)
            if self._baseinstance is not None:
                setattr(self._baseinstance, name, widget)
        return widget

    def loadUi(self, uifile, baseinstance=None):
        self._baseinstance = baseinstance
        widget = self.load(uifile)
        QtCore.QMetaObject.connectSlotsByName(widget)
        return widget


class MainWindow(QMainWindow):
    def __init__(self, parent=None):
        super(MainWindow, self).__init__(parent)

        # from main_ui import Ui_MainWindow
        # self.ui = Ui_MainWindow()
        # self.ui.setupUi(self)

        # 导入我们生成的界面【动态加载ui文件,QMainWind会出现异常，需要重写UiLoader】
        self.ui = UiLoader().loadUi("main.ui", self)

        # 从ui属性里可以访问界面中的对象
        # 这里用到了Qt的信号-槽机制，以后会提到
        # 现在只需要记住，action的triggered属性代表被点击
        # 使用connect()方法可以设置被点击后执行的方法
        # 点击action1就会执行trigger_action1()
        self.ui.action1.triggered.connect(self.trigger_action1)
        # 点击action2就会执行trigger_action2()
        self.ui.action2.triggered.connect(self.trigger_action2)

    def trigger_action1(self):
        # 使用setText()方法设置Label显示的文本
        self.ui.label.setText("动作1")
        # 使用showMessage()方法可以临时显示状态信息
        self.ui.statusbar.showMessage("你点击了动作1")

    def trigger_action2(self):
        self.ui.label.setText("动作2")
        self.ui.statusbar.showMessage("你点击了动作2")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    # 结束QApplication
    sys.exit(app.exec_())
    # 注意，在PySide6中，需要使用app.exec()
    # sys.exit(app.exec())
```

​		我们执行这个py文件，在显示的界面中点击菜单1中的选项1或选项2，会发现Label和状态栏显示的文字会发生变化，而状态栏显示的文字会随着鼠标再次移动到菜单栏上而消失（不是bug，状态栏的设计就是这样的）：

![image-20231226134849353](imge/Pyside2学习.assets/image-20231226134849353.png "执行结果")









# 遇到的问题

---



## 1、ValueError: source code string cannot contain null bytes

通过 pyside2-uic hello.ui > hello_ui.py 生成py文件（1文件），运行后报 ValueError: source code string cannot contain null bytes 错误。

![image-20231226132623107](imge/Pyside2学习.assets/image-20231226132623107.png)

通过 pyside2-uic 工具生成的 py文件（2文件） 正常运行。

对比以上2个文件内容完全一样，通过以下代码，发现1文件编码格式为 UTF-16 ， 2文件编码格式为 ascii 

```python
from chardet import detect

file_path="main_ui.py"
with open(file_path, 'rb') as f:
    # 获取文件编码格式
    encode = detect(f.read(10))['encoding']
    print(encode)
```

问题原因：文件编码格式原因，导致的报错。

## 2、PySide2 QUiLoader导致主窗口（mainwindow）异常

主窗口 相关控件不能正常显示。

重写这个加载工具，重写后的UiLoader如下：

```python
from PySide2 import QtGui, QtCore, QtUiTools

class UiLoader(QtUiTools.QUiLoader):
    _baseinstance = None

    def createWidget(self, classname, parent=None, name=''):
        if parent is None and self._baseinstance is not None:
            widget = self._baseinstance
        else:
            widget = super(UiLoader, self).createWidget(classname, parent, name)
            if self._baseinstance is not None:
                setattr(self._baseinstance, name, widget)
        return widget

    def loadUi(self, uifile, baseinstance=None):
        self._baseinstance = baseinstance
        widget = self.load(uifile)
        QtCore.QMetaObject.connectSlotsByName(widget)
        return widget
```


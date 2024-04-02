#  一、Pyside6模块安装

```cmd
pip install Pyside6
```

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

pyside6-rcc:
    名称：任意即可（建议qrc2py,这样一目了然）
    程序：程序所在目录
    参数：$FileName$ -o $FileNameWithoutExtension$_rc.py
    工作目录：$FileDir$

pyside6-uic:
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

## 工具配置：

![image-20240329221052861](imge/PySide6.assets/image-20240329221052861.png)

# 三、基本使用

## PySide6启动方式

```python
import sys
from PySide6.QtWidgets import QApplication, QPushButton, QWidget


class MainWindow(QWidget):
    def __init__(self, parent=None):
        super(MainWindow, self).__init__(parent)
        self.setGeometry(300, 300, 300, 300)
        self.setWindowTitle('启动方式1')
        button = QPushButton('Close', self)
        button.clicked.connect(self.close)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    win = MainWindow()
    win.show()
    sys.exit(app.exec())
```

**效果：**

![image-20240329222559769](imge/PySide6.assets/image-20240329222559769.png)

**PyQt5参考内容：**

![image-20240330075647297](imge/PySide6.assets/image-20240330075647297.png)

# 四、QT Designer的使用

## 4.1 快速入门

### 属性说明

![image-20240329224301866](imge/PySide6.assets/image-20240329224301866.png)

![image-20240329224604659](imge/PySide6.assets/image-20240329224604659.png)

- geometry属性：[参见](#4.2.1 绝对布局 "4.2.1 绝对布局")
- minimumSize和maximumSize属性：[参见](#4、minimumSize和maximumSize属性  "4.2.2 4、minimumSize和maximumSize属性")
- sizePollcy属性：[参见](#5、sizePolicy属性  "4.2.2 5、sizePolicy属性")

### 转换ui文件

- 命令行 

  ```cdm
  pyside6-uic.exe -o firstMainWin.py firstMainWin.ui
  ```

- Pycharm外部工具

- python脚本

  ui2py.py

  ```python
  import os
  import os.path
  
  
  # 列出目录下的所有.qrc文件
  def list_ui_file(filepath):
      _list = []
      files = os.listdir(filepath)
      for filename in files:
          # os.sep 显示当前平台下路径分隔符
          # print(filepath + os.sep + filename)
          if os.path.splitext(filename)[1] == '.qrc':
              _list.append(filename)
  
      return _list
  
  
  # 把后缀为.qrc的文件名改成后缀为_qrc.py的文件名
  def trans_py_file(filename):
      return os.path.splitext(filename)[0] + '_qrc.py'
  
  
  # 调用系统命令把.qrc文件转换为.py
  def run_main(qrc_path):
      """
       把 .qrc 文件转换为 .py文件
      :param qrc_path: .qrc文件所在路径
      :return:运行结果,0 表示运行成功， 1 表示运行失败
      """
      list_ = list_ui_file(qrc_path)
      for qrc_file in list_:
          # 相对路径转换成绝对路径
          qrc_file = os.path.join(os.path.abspath(qrc_path), qrc_file)
          py_file = trans_py_file(qrc_file)
          # PyQt 6 适用
          # cmd = 'pyuic6 -o {py_file} {qrc_file}'.format(py_file=py_file, qrc_file=qrc_file)
  
          # PySide 6 适用
          cmd = 'pyside6-rcc -o "{py_file}" "{qrc_file}" '.format(py_file=py_file, qrc_file=qrc_file)
          print(cmd)
          # 如果要直接看到运行结果的话，应该使用os.system；
          # 如果需要获取返回值做进一步的处理则使用os.popen
          return os.system(cmd)
  
  
  # ########### 程序的主入口
  if __name__ == '__main__':
      t = run_main("../ui")
      print(t)
  
  ```

### 转换qrc文件

[同 转换ui文件类似，使用 pyside6-rcc.exe](#转换ui文件)

### 界面与逻辑分离

.ui 转换的 py 文件 是界面文件，调用界面文件的 py 文件是逻辑文件。

#### 静态加载

```python
import sys
from PySide6.QtWidgets import QApplication, QPushButton, QMainWindow
from ui.firstMainWin_ui import Ui_MainWindow


class MyMainWindow(QMainWindow, Ui_MainWindow):
    def __init__(self, parent=None):
        super(MyMainWindow, self).__init__(parent)
        self.setupUi(self)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    win = MyMainWindow()
    win.show()
    sys.exit(app.exec())
```

#### 动态加载，不推荐

**注意：**PySide主窗口 相关控件不能正常显示,需要重写加载工具

```python
import sys
from PySide6.QtWidgets import QApplication, QMainWindow
from PySide6 import QtCore, QtUiTools


class UiLoader(QtUiTools.QUiLoader):
    """
    PySide主窗口 相关控件不能正常显示,重写加载工具
    """
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


class MyMainWindow(QMainWindow):
    def __init__(self, parent=None):
        super(MyMainWindow, self).__init__(parent)
        self.ui = UiLoader().loadUi("ui/firstMainWin.ui", self)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    win = MyMainWindow()
    win.show()
    sys.exit(app.exec())

```

## 4.2  布局管理

QT Designer提供了4种窗口布局方式：

- Vertical Layout(垂直布局)：控件默认按照从上到下的顺序进行纵向添加

- Horizontal Layout(水平布局)：控件默认按照从左到右的顺序进行横向添加

- Grid Layout(网格布局)：

  > 先将窗口控件放入一个网格中，然后将它们合理地划分成若干行（row）和列（column），并把其中的每个窗口控件放置在合适的单元（cell）中，这里的单元指由行和列交叉所划分出来的空间

- Form Layout(表单布局)：控件以两列的形式布局在表单中，其中左列包含标签，右列包含输入控件

  ```
  # Form Layout(表单布局)：控件以两列的形式布局在表单中，
  # 其中左列包含标签，右列包含输入控件
  flo = QFormLayout()
  
  flo.addRow("普通文本框，居中", lineEdit_normal)
  flo.addRow(button, lineEdit_edit)
  self.setLayout(flo)
  ```

  

![image-20240330080810541](imge/PySide6.assets/image-20240330080810541.png)

常用布局一般有两种方式：

- 使用布局管理器
- 使用容器控件

### 4.2.1 绝对布局

最简单的布局方法就是设置geometry属性，主要用来设置控件在窗口中的绝对坐标与控件自身大小

![image-20240330120131305](imge/PySide6.assets/image-20240330120131305.png)

```python
# 创建PushButton 对象，父类self.centralwidget
self.pushButton = QPushButton(self.centralwidget)
# 设置对象位置及大小
self.pushButton.setGeometry(QRect(20, 10, 75, 24))
# 设置对象名字
self.pushButton.setObjectName(u"pushButton")
```



![image-20240330122334758](imge/PySide6.assets/image-20240330122334758.png)

```python
self.label = QLabel(self.centralwidget)
self.label.setObjectName(u"label")
self.label.setGeometry(QRect(82, 244, 24, 16))

self.doubleSpinBox_returns_min = QDoubleSpinBox(self.centralwidget)
self.doubleSpinBox_returns_min.setObjectName(u"doubleSpinBox_returns_min")
self.doubleSpinBox_returns_min.setGeometry(QRect(138, 244, 53, 20))
```

### 4.2.2 布局管理器布局

- QMainWindow窗口中添加布局管理器

  QMainWindow 不能用于设置布局（使用setLayout()函数），因为主窗口的程序默认已经有了自己的布局管理器。每个QMainWindow类都有一个中心控件QWidget（中心窗口），可以对该QWidget布局来实现对QMainWindow的布局，中心控件由setCentralWidget来添加。

  ```python
  class MainWidget(QMainWindow):
      def __init__(self, parent=None):
          super(MainWidget, self).__init__(parent)
  		...
          # 添加布局管理器
          layout = QVBoxLayout()
          widget = QWidget(self)
          widget.setLayout(layout)
          # Geometry 相对坐标系,设置widget位置及大小
          widget.setGeometry(QtCore.QRect(200, 150, 200, 200))
          # self.setCentralWidget(widget)
          self.widget = widget
  
          # 关闭窗口
          self.button1 = QPushButton('关闭主窗口')
          self.button1.clicked.connect(self.close)
          layout.addWidget(self.button1)
  ```

- QWidget窗口中添加布局管理器

  ```python
  
  class WindowLabel(QWidget):
      def __init__(self):
          super().__init__()
  		...
          # 添加布局管理
          vbox = QVBoxLayout()
          self.setLayout(vbox)
          # 以上两行代码 也可以写成  vbox = QVBoxLayout(self)
          vbox.addWidget(label_normal)
          
          # vbox.addStretch()
          
  ```

- QDialog 窗口中添加布局管理器

  ```python
  mainLayout = QGridLayout(self)
  mainLayout.addWidget(nameLb1, 0, 0)
  ```

  

#### 1、垂直布局器 QVBoxLayout()

![image-20240330123529412](imge/PySide6.assets/image-20240330123529412.png)

![image-20240330125047706](imge/PySide6.assets/image-20240330125047706.png)

```
# 创建 垂直布局器 
self.verticalLayout = QVBoxLayout()
self.verticalLayout.setObjectName(u"verticalLayout")

self.verticalLayout.setContentsMargins(0, 0, 0, 0)
self.verticalLayout.addWidget(self.label_6)
self.verticalLayout.addWidget(self.label)
self.verticalLayout.addWidget(self.label_2)
self.verticalLayout.addWidget(self.label_3)

# 下面参数可不设置，保持默认
self.verticalLayout.setStretch(1, 2)
self.verticalLayout.setStretch(3, 1)

self.verticalLayout.setSizeConstraint(QLayout.SetMinimumSize)

self.verticalLayout.setSpacing(6)
```

#### 2、网格布局 QGridLayout()

![image-20240330130218866](imge/PySide6.assets/image-20240330130218866.png)

![image-20240330131158371](imge/PySide6.assets/image-20240330131158371.png)

```python
# 创建网格布局器
self.gridLayout = QGridLayout()
self.gridLayout.setObjectName(u"gridLayout")
self.gridLayout.setContentsMargins(0, 0, 0, 0)

# gridLayout.addWidget(窗口控件, 行位置, 列位置, 要合并的行数, 要合并的列数)，后两个是可选参数
self.gridLayout.addWidget(self.label_4, 0, 0, 1, 1)
self.gridLayout.addWidget(self.label_5, 0, 1, 1, 1)
self.gridLayout.addWidget(self.doubleSpinBox_returns_min, 1, 0, 1, 1)
self.gridLayout.addWidget(self.doubleSpinBox_returns_max, 1, 1, 1, 1)
self.gridLayout.addWidget(self.doubleSpinBox_maxdrawdown_min, 2, 0, 1, 1)
self.gridLayout.addWidget(self.doubleSpinBox_maxdrawdown_max, 2, 1, 1, 1)
self.gridLayout.addWidget(self.doubleSpinBox_sharp_min, 3, 0, 1, 1)
self.gridLayout.addWidget(self.doubleSpinBox_sharp_max, 3, 1, 1, 1)

# 下面参数可不设置，保持默认
self.gridLayout.setSizeConstraint(QLayout.SetMinimumSize)
self.gridLayout.setHorizontalSpacing(33)
self.gridLayout.setVerticalSpacing(11)

self.gridLayout.setRowStretch(0, 4)
self.gridLayout.setRowStretch(1, 6)
self.gridLayout.setRowStretch(2, 2)
self.gridLayout.setRowStretch(3, 1)

self.gridLayout.setColumnStretch(0, 4)
self.gridLayout.setColumnStretch(1, 8)

self.gridLayout.setColumnMinimumWidth(0, 3)
self.gridLayout.setColumnMinimumWidth(1, 6)

self.gridLayout.setRowMinimumHeight(1, 5)
self.gridLayout.setRowMinimumHeight(3, 1)
```

#### 3、水平布局 QHBoxLayout()

![image-20240330132410340](imge/PySide6.assets/image-20240330132410340.png)

- Vertical Spacer 表示两个布局器不要彼此挨着
- Horizontal Spacer 表示”开始“按钮与网格布局器尽可能离远一点
- Vertical Line 用一条线分割开来

![image-20240330135153955](imge/PySide6.assets/image-20240330135153955.png "水平间隔设置")

```python
self.line = QFrame(self.widget)
self.line.setObjectName(u"line")
self.line.setFrameShape(QFrame.VLine)
self.line.setFrameShadow(QFrame.Sunken)
# 垂直间隔设置
self.verticalSpacer = QSpacerItem(20, 40, QSizePolicy.Policy.Minimum, QSizePolicy.Policy.Expanding)
# 水平间隔设置
self.horizontalSpacer = QSpacerItem(13, 108, QSizePolicy.Policy.MinimumExpanding, QSizePolicy.Policy.Minimum)
```

![image-20240330135054667](imge/PySide6.assets/image-20240330135054667.png)

设置方法参考垂直布局

#### 4、minimumSize和maximumSize属性

用来控制布局管理器中的最小尺寸和最大尺寸。



![image-20240330140232185](imge/PySide6.assets/image-20240330140232185.png)

```python
self.pushButton.setMinimumSize(QSize(100, 100))
self.pushButton.setMaximumSize(QSize(300, 300))
```

无论如何压缩按钮都不会小于100像素，无论如何拉伸也不会大于300像素。

![image-20240330140817070](imge/PySide6.assets/image-20240330140817070.png)

#### 5、sizePolicy属性

每个窗口控件都有属于自己的两个尺寸：

- sizeHint（推荐尺寸）：窗口控件的期望尺寸
- minimumSizeHint（推荐最小尺寸）：对窗口控件进行压缩时能被压缩到的最小尺寸。

如果控件没有被布局，那么这两个函数返回无效值，否则返回对应尺寸。没有被布局的控件不建议使用这两个函数，若控件已经被布局，除非设置了minimumSize或将sizePolicy属性设置为QsizePolicy.Ignore，否则控件尺寸不会小于minimumSizeHint。

sizePolicy属性作用是如果窗口控件在布局管理器中的布局不能满足我们的需求，可以通过设置该窗口控件的sizePolicy属性实现布局的微调。sizePolicy是每个窗口控件特有属性，不同窗口控件可能不同。

按钮控件默认的sizePolicy属性的设置：

![image-20240330142917213](imge/PySide6.assets/image-20240330142917213.png)

```python
# 策略
sizePolicy = QSizePolicy(QSizePolicy.Policy.Minimum, QSizePolicy.Policy.Fixed)
# 水平伸展 0
sizePolicy.setHorizontalStretch(0)
# 垂直伸展 0
sizePolicy.setVerticalStretch(0)
sizePolicy.setHeightForWidth(self.pushButton.sizePolicy().hasHeightForWidth())

self.pushButton.setSizePolicy(sizePolicy)
```

水平策略和垂直策略参数：

- Fixed：窗口控件具有其sizeHint所提示的尺寸，并且尺寸不会再改变
- Minimum：窗口控件的sizeHint所提示的尺寸就是它最小的尺寸，不能被压缩比这个值小，可以扩展更大，没有优势
- Maximum：窗口控件的sizeHint所提示的尺寸就是它最大的尺寸，不能比这个值大，可以缩小
- Preferred：窗口控件的sizeHint所提示的尺寸就是它的期望尺寸，控件可以缩小、变大，和其它控件的sizeHint（默认QWidget的策略）相比没有优势
- Expanding：窗口控件可以缩小到minimumsizeHint所提示的尺寸，也可以变大比sizeHint所提示的尺寸大，它希望能够变得更大
- MinimumExpanding：窗口控件的sizeHint所提示的尺寸就是它最小的尺寸，控件不能被压缩的比这个值小，它希望能够变得更大
- Ignored：无视窗口控件的sizeHint和minimumsizeHint所提示的尺寸，控件将获得尽可能多的空间

3个标签会分别按照1:3:1比例缩放。

![image-20240330145120541](imge/PySide6.assets/image-20240330145120541.png)

```python
sizePolicy = QSizePolicy(QSizePolicy.Policy.Preferred, QSizePolicy.Policy.Preferred)
sizePolicy.setHorizontalStretch(0)
sizePolicy.setVerticalStretch(1)
sizePolicy.setHeightForWidth(self.label.sizePolicy().hasHeightForWidth())
self.label.setSizePolicy(sizePolicy)

sizePolicy1 = QSizePolicy(QSizePolicy.Policy.Preferred, QSizePolicy.Policy.Preferred)
sizePolicy1.setHorizontalStretch(0)
sizePolicy1.setVerticalStretch(3)
sizePolicy1.setHeightForWidth(self.label_2.sizePolicy().hasHeightForWidth())
self.label_2.setSizePolicy(sizePolicy1)

sizePolicy.setHeightForWidth(self.label_3.sizePolicy().hasHeightForWidth())
self.label_3.setSizePolicy(sizePolicy)
```



##### sizePolicy:控件的布局（参考）

参见：https://blog.csdn.net/kongcheng253/article/details/128769765

![image-20231227145005865](imge/PySide6.assets/image-20231227145005865.png "sizePolicy属性")

基于 QWidget的控件都会继承 sizePolicy 属性（ QSizePolicy 类型），这个属性包括两个大的方面内容：**伸展因子 （Stretch Factor）和 伸展策略（Policy）**，这些都会影响到界面最终的布局显示

伸展因子（Stretch Factor）：

---

> 水平伸展、垂直伸展

```
取值范围是 0 到 255，负数就当做 0，大于 255 就当做 255，因此设置超出范围的数也没意义。
```

伸展因子都是 0，那么三个按钮在水平布局里就是均匀拉伸：

![image-20231227150038184](imge/PySide6.assets/image-20231227150038184.png "伸展因子都是 0")

如果把 "One" 按钮的 "水平伸展" 设为 1，"Two" 按钮的 "水平伸展" 设为 2，"Three" 按钮的 "水平伸展" 设为  3，那么在窗口拉大时，该行三个按钮的伸展因子之和为 1+2+3 == 6，新的空间就按照 1/6 ，2/6 ，3/6  的比例划分给这三个按钮，显示效果就如下面这样:

![image-20231227150119818](imge/PySide6.assets/image-20231227150119818.png "伸展因子 1、2、3")

如果把 "One" 按钮的 "水平伸展" 设为 2，"Two" 按钮的 "水平伸展" 设为 4，"Three" 按钮的 "水平伸展" 设为  0，那么在窗口拉大时，分配规律就是：先计算伸展因子之和 2+4+0 == 6，新的空间按照 2/6 ，4/6，0/6  的比例划分给这三个按钮，显示效果如下：

![image-20231227150311451](imge/PySide6.assets/image-20231227150311451.png "伸展因子 2、4、0")

因为第三个按钮的伸展因子是 0，第三个按钮会保持一个建议尺寸，其他两个按钮会根据伸展因子的占比进行拉伸。三个水平伸展因子为 2、4、0，其实也可以直接写成 1、2、0，两种是等价的，不管有没有公约数

**除了控件自身可以设置伸展因子，布局器也可以为内部直属的控件或子布局器设置伸展因子。如果布局器和内部直属的控件都设置了伸展因子，那么布局器的设置会覆盖直属控件的伸展因子。因此==不建议==直接设置控件自己的伸展因子属性，而是通过布局器来设置各个子控件或子布局器的伸展因子。**

伸展策略：

---

> 水平策略、垂直策略

| 枚举常量         | **数值**                               | **拉伸特点** | **描述**                                                     |
| ---------------- | -------------------------------------- | ------------ | ------------------------------------------------------------ |
| Fixed            | 0                                      | 固定         | 以建议尺寸固定住，对于水平方向是固定宽度，垂直方向是固定高度。 |
| Minimum          | GrowFlag                               | 被动拉大     | 以建议尺寸为最小尺寸，如果有多余的空间就拉伸，没有多余的空间就保持建议尺寸。被动扩张。 |
| Maximum          | ShrinkFlag                             | 被动缩小     | 以建议尺寸为最大尺寸，窗口缩小时，如果其他控件需要，该控件可以尽量缩小为其他控件腾出空间。 |
| Preferred        | GrowFlag \|  ShrinkFlag                | 被动伸缩     | 以建议尺寸为最佳尺寸，能屈能伸，窗口缩小时可以为其他控件腾出空间，窗口变大时，也可以占据其他控件不需要的空闲空间。基类 QWidget 默认是这种策略。被动扩张。 |
| Expanding        | GrowFlag \|  ShrinkFlag \|  ExpandFlag | 主动扩张     | 建议尺寸仅仅是明智的建议，但控件基本不采用。这个模式也是能屈能伸，但它倾向于主动扩张，它会尽可能占据新增的区域。 |
| MinimumExpanding | GrowFlag \|  ExpandFlag                | 主动扩张     | 以建议尺寸作为最小尺寸，主动扩张，尽可能占据新增的区域。     |
| Ignored          | ShrinkFlag \|  GrowFlag \|  IgnoreFlag | 野蛮扩张     | 忽略建议尺寸，虽然能屈能伸，但是它会尽最大可能占据空间。     |

![image-20231227152832619](imge/PySide6.assets/image-20231227152832619.png "水平策略示例")





### 4.2.3 Qt DesIgner布局顺序

使用Qt Designer开发一个完整的GUI程序的流程如下：

1、将一个窗口控件拖拽到窗口中并放置在大致正确的位置，除了Containers栏，一般不需要调整各栏的尺寸

2、要用代码引用的窗口控件应指定一个名字，需要微调的窗口控件可以设置对应的属性

3、重复前两个步骤，直到所需要的全部窗口控件都被拖拽到窗口中

4、如有需要，在窗口控件之间可以用 Vertical Spacer、Horizontal Spacer、Horizontal Line、Vertical Line隔开（实际上前两个步骤就可以包含这部分内容）

5、选择需要布局的窗口控件，使用布局管理器或切分窗口（splitter）对他们布局

6、重复步骤 5 ，直到所有的窗口控件和分隔符都布局好为止

7、单击窗口，并使用布局管理器对其进行布局

8、为窗口中的标签设置伙伴关系

9、如果按键次序有问题，则需要设置窗口的Tab健次序

10、在适当的地方为内置信号和槽建立信号与槽连接

11、预览窗口，并检查所有内容能否按照设想进行工作

12、设置窗口的对象名（在类中会用到这个名字）、窗口的标题并保存

13、使用工具pyside6-uic.exe编译窗口，并根据需要生成对话框代码（在逻辑文件上建立信号与槽连接的方式）

14、进行正常的代码编写工作，即编写业务逻辑文件。

### 4.2.4 设置伙伴关系

标签控件里的 "&" 用于设置伙伴快捷键，因为单行编辑控件没法显示自己的快捷键，所以需要通过伙伴标签控件来设置快捷键。

"&MAC" 意味着伙伴快捷键为 Alt+M ，"&IP" 快捷键就是 Alt+I ，"&Port" 快捷键是 Alt+P 。

当然，快捷键能实现的前提是设置伙伴，我们点击设计模式上面的带有橙色小块的图标，进入伙伴编辑模式：

![image-20231229111056479](imge/PySide6.assets/image-20231229111056479.png "编辑伙伴模式")

在伙伴编辑模式，编辑伙伴关系类似在画图板画线的操作，从标签控件画线到右边的单行编辑控件即可。

将三行的标签都设置为对应的单行编辑控件伙伴。

设置为伙伴之后， 标签控件就不再显示 "&" ，而是将 "&" 右边第一个==字母添加下划线显示==，这样伙伴快捷键就设置成功了。

程序运行时，伙伴快捷键自动生效：
按 Alt+M ，自动切换到 MAC 地址编辑控件；
按 Alt+I ，自动切换到 IP 地址编辑控件；
按 Alt+P ，自动切换到端口编辑控件。
示范的例子标签文本都是英文的，如果是中文文本，以端口为例，可以设置为 "端口(&P)" ，这样快捷键也是 Alt+P。

对应代码：

```PYTHON
self.label.setBuddy(self.doubleSpinBox_returns_min)
```

### 4.2.5 设置Tab键次序

![image-20240330202901521](imge/PySide6.assets/image-20240330202901521.png)

进入编辑Tab键次序模式，两种修改方式：

- 按顺序单击可修改
- 右键菜单-制表符顺序列表调整

## 4.3 信号与槽关联

信号/槽是Qt的核心机制。在创建时间循环之后，通过建立信号与槽的连接就可以实现对象之间的通信。当信号发射（Emit）时，连接的槽函数将自动执行。在PySide中，信号与槽通过QObject.signal.connect()连接。

从QObject类或其子类（如QWidget）派生的类都能够包含信号与槽，当对象改变其状态时，信号就由该对象发射出去。槽用于接收信号，但槽函数是普通的对象成员函数。

在Qt编程中，通过Qt信号/槽机制对鼠标或键盘在界面上的操作进行相应处理。不同的控件能够发射的信号种类和触发时机不相同，在Qt的文档中有说明。

为控件发射的信号指定对应的处理槽函数有2种方法：

- 在Qt Designer中添加信号与槽
- 通过代码连接信号与槽

### 4.3.1 简单入门

- 在Qt Designer中添加信号与槽

![image-20240330210821667](imge/PySide6.assets/image-20240330210821667.png)

- 在Qt Designer中添加信号与槽

![image-20240330210923136](imge/PySide6.assets/image-20240330210923136.png)

```python
# clicked 当鼠标左键被按下然后释放时或者快捷键被释放时触发该信号
self.closeWinBtn.clicked.connect(Form.close)
# pressed 信号 当鼠标指针在按钮上并按下左键时触发该信号
self.closeWinBtn.pressed.connect(Form.testSlot)
# 通过pyside-uic.exe编译后有下面的代码
#表示根据名字连接信号与槽
QMetaObject.connectSlotsByName(Form)
```

**调用窗口**

MainWinSignalSlog1Run.py

```python
import sys
from PySide6.QtWidgets import QApplication, QMainWindow
from MainWinSignalSlog1_ui import Ui_Form


class MyMainWindow(QMainWindow, Ui_Form):
    def __init__(self, parent=None):
        super(MyMainWindow, self).__init__(parent)
        self.setupUi(self)

    def testSlot(self):
        print("这是一个自定义函数，您成功了")


if __name__ == '__main__':
    app = QApplication(sys.argv)
    myWin = MyMainWindow()
    myWin.show()
    sys.exit(app.exec())
```

### 4.3.2 获取信号与槽

#### 1、从Qt Designer中获取信号与槽

显示控件可用的信号与槽函数，可自定义添加

![image-20240330213351179](imge/PySide6.assets/image-20240330213351179.png)

#### 2、使用官方帮助网站获取信号与槽

官方网址：https://doc.qt.io/qt-6/classes.html

![image-20240330220434060](imge/PySide6.assets/image-20240330220434060.png "网页展示")

**获取控件所属模块**

通过导入的控件 反向查找父类

![image-20240330220552123](imge/PySide6.assets/image-20240330220552123.png)



![image-20240330220808313](imge/PySide6.assets/image-20240330220808313.png)

**信号与槽的详细介绍**

![image-20240330221233542](imge/PySide6.assets/image-20240330221233542.png)

### 4.3.3 使用信号/槽机制

## 4.4 菜单栏与工具栏

### 4.4.1 界面设计

![image-20240330224233712](imge/PySide6.assets/image-20240330224233712.png)

#### 1、添加快捷键

- 文件、编辑菜单:通过输入 文件(&F)、编辑(&E) 创建快捷键

- 新建、打开、关闭：通过 动作编辑器 或属性编辑器 中的Shortcut 创建快捷方式

  ![image-20240330224718219](imge/PySide6.assets/image-20240330224718219.png)

  ![image-20240330225325472](imge/PySide6.assets/image-20240330225325472.png)

![image-20240330224744930](imge/PySide6.assets/image-20240330224744930.png)

#### 2、工具栏

默认不显示工具栏，可通过右键添加工具栏，通过拖拽可将动作编辑器中的动作添加到工具栏。

![image-20240330225256834](imge/PySide6.assets/image-20240330225256834.png)

### 4.4.2 效果测试

```python
import sys, os
from PySide6.QtWidgets import QMainWindow, QApplication, QFileDialog
from MainWinMenuToolbar_ui import Ui_MainWindow


class MainWinMenuToolbarRun(QMainWindow, Ui_MainWindow):
    def __init__(self, parent=None):
        super(MainWinMenuToolbarRun, self).__init__(parent)
        self.setupUi(self)
        # 菜单的单击事件,当单击关闭菜单时连接槽函数close()
        self.fileCloseAction.triggered.connect(self.close)
        # 菜单的单击事件,当单击打开菜单时连接槽函数 openMsg
        self.fileOpenAction.triggered.connect(self.openFile)
        # 打开计算器
        self.openCalc.triggered.connect(lambda: os.system('calc'))
        # 打开记事本
        self.openNotepad.triggered.connect(lambda: os.system('notepad'))

    def openFile(self):
        file, ok = QFileDialog.getOpenFileName(self, '打开', "C:/", "ALL Files (*);;Text Files (*.txt)")
        # 在状态栏中显示文件地址
        self.statusbar.showMessage(file)
        print(ok)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    myWin = MainWinMenuToolbarRun()
    myWin.show()
    sys.exit(app.exec())
```



![image-20240330232522255](imge/PySide6.assets/image-20240330232522255.png)

## 4.5 添加图片

引用图片资源主要有两种方法：

1. 先将资源文件转换为python文件，然后引用python文件（PyQt6 不支持）
2. 在程序中通过相对路径引用外部资源

### 4.5.1 创建资源文件

![image-20240331150841690](imge/PySide6.assets/image-20240331150841690.png)

按照以上步骤添加图片后，apprcc.qrc文件内容如下：

```xml
<RCC>
  <qresource prefix="pic">
    <file>images/calc.jpg</file>
    <file>images/close.jpg</file>
    <file>images/new.jpg</file>
    <file>images/notepad.jpg</file>
    <file>images/open.jpg</file>
    <file>images/python.jpg</file>
  </qresource>
</RCC>
```

### 4.5.2 添加资源文件

#### 1、为菜单栏和工具栏添加图标

对fileOpenAction、fileNewAction 使用 ==选择资源==，对fileCloseAction使用 ==选择文件==

![image-20240331153647008](imge/PySide6.assets/image-20240331153647008.png)

```python
icon = QIcon()
icon.addFile(u":/pic/images/open.jpg", QSize(), QIcon.Normal, QIcon.Off)
self.fileOpenAction.setIcon(icon)


icon1 = QIcon()
icon1.addFile(u"images/close.jpg", QSize(), QIcon.Normal, QIcon.Off)
self.fileCloseAction.setIcon(icon1)

icon2 = QIcon()
icon2.addFile(u":/pic/images/new.jpg", QSize(), QIcon.Normal, QIcon.Off)
self.fileNewAction.setIcon(icon2)
```

![image-20240331153717682](imge/PySide6.assets/image-20240331153717682.png)

#### 2、在窗体中添加图片

窗体中添加Label控件，通过 pixmap 属性添加图片

![image-20240331154327124](imge/PySide6.assets/image-20240331154327124.png)

#### 3、转换资源文件

[见](#转换qrc文件 "转换方法")

导入方法,ui文件转换成py文件时会自动导入

```python
import apprcc_rc
```

# 五、基本窗口控件

基本控件：一些简单的、容易使用的控件，主要是单一控件，可以呈现简单信息

高级控件：相对复杂一些控件，如表格与多窗口（页面）控件，可以显示更多、更复杂的信息

## 5.1 主窗口（QMainWindow/QWidget/QDialog）

主窗口为用户提供了一个应用框架。它有自己的布局，可以在布局中添加控件。

### 5.1.1 窗口类型

Qt Designer可以创建3种窗口：

- Dialog 对应的类 QDialog ，是对话窗口的基类，对话框用来执行短期任务、与用户进行互动，可以是模态的也可以是非模态的
- Widget 对应的类 QWidget，主要用于嵌入窗口以及作为多窗口应用的子窗口，也可以作为主窗口使用
- Main Window  对应的类 QMain Window， 包含菜单栏、工具栏、状态栏、标题栏等，是GUI程序主窗口

使用原则如下：

- 如果是主窗口，则使用QMain Window类
- 如果是对话框，则使用QDialog类
- 如果是嵌入窗口，则使用QWidge类

### 5.1.2 创建主窗口

一个程序包含一个或多个窗口或控件，必定有一个窗口是其它窗口的父类，将这个窗口成为主窗口（或顶层窗口）。

其它窗口或控件继承主窗口，方便对他们进行管理，在需要的时候启动，不需要的时候删除。

主窗口一般是QMainWindow的实例，用一个控件（QWidget）占位符来占着中心窗口，可以使用setCentralWidget()函数来设置中心窗口。

![image-20240331164242563](imge/PySide6.assets/image-20240331164242563.png)

```python
self.centralwidget = QWidget(MainWindow)
self.centralwidget.setObjectName(u"centralwidget")
MainWindow.setCentralWidget(self.centralwidget)
```

QMainWindow中重要的函数：

| 函数               | 描述                                                         |
| :----------------- | ------------------------------------------------------------ |
| addToolBar()       | 添加工具栏                                                   |
| centralWidget()    | 返回中心窗口的一个控件，未设置时返回NULL                     |
| menuBar()          | 返回主窗口的菜单栏                                           |
| setCentralWidget() | 设置中心窗口的控件                                           |
| setStatusBar()     | 设置状态栏                                                   |
| statusBar()        | 获得状态栏对象后，调用状态栏对象的showMessage(message,int timeout=0)方法显示状态栏信息。<br>第一个参数：要显示的状态栏信息；<br>第二个参数：信息停留的时间，单位是毫秒，默认是0，表示一直显示状态栏信息 |

**注意：**

QMainWindow 不能用于设置布局（使用setLayout()函数），因为主窗口的程序默认已经有了自己的布局管理器。每个QMainWindow类都有一个中心控件QWidget（中心窗口），可以对该QWidget布局来实现对QMainWindow的布局，中心控件由setCentralWidget来添加。

```python
# 添加布局管理器
layout = QVBoxLayout()
widget = QWidget(self)
widget.setLayout(layout)
self.setCentralWidget(widget)
```

#### 案例：

```python
import sys

from PySide6.QtGui import QGuiApplication, QIcon
from PySide6.QtWidgets import (QApplication,
                               QMainWindow,
                               QVBoxLayout,
                               QWidget, QPushButton, )
from PySide6 import QtCore


class MainWidget(QMainWindow):
    def __init__(self, parent=None):
        super(MainWidget, self).__init__(parent)
        # 设置主窗口标签
        self.setWindowTitle('QMainWindow 例子')
        self.resize(800, 400)
        # 状态栏
        self.status = self.statusBar()

        # 添加布局管理器
        layout = QVBoxLayout()
        widget = QWidget(self)
        widget.setLayout(layout)
        # Geometry 相对坐标系
        widget.setGeometry(QtCore.QRect(200, 150, 200, 200))
        # self.setCentralWidget(widget)
        self.widget = widget

        # 关闭窗口
        self.button1 = QPushButton('关闭主窗口')
        self.button1.clicked.connect(self.close)
        layout.addWidget(self.button1)

        # 主窗口居中显示
        self.button2 = QPushButton('主窗口居中')
        self.button2.clicked.connect(self.center)
        layout.addWidget(self.button2)

        # 显示图标
        self.button3 = QPushButton('显示图标')
        self.button3.clicked.connect(lambda: self.setWindowIcon(QIcon("../images/cartoon1.ico")))
        self.button3.clicked.connect(lambda: self.button3.setIcon(QIcon("../images/cartoon2.ico")))
        layout.addWidget(self.button3)
        # 显示状态栏
        self.button4 = QPushButton('显示状态栏')
        self.button4.clicked.connect(
            lambda: self.status.showMessage("这是我的状态栏提示，5秒钟后消失", 5000))
        layout.addWidget(self.button4)

        # 显示窗口坐标和大小
        self.button5 = QPushButton('显示窗口坐标及大小')
        self.button5.clicked.connect(self.show_geometry)
        layout.addWidget(self.button5)

    def center(self):
        # 获取的屏幕大小是被缩放过的大小，比如我电脑125%的缩放，
        # 1920*1080的屏幕大小，在程序中获得的屏幕大小是1536*864，
        # 需要还原的话直接乘个缩放倍数1.25就行。
        screen = QGuiApplication.primaryScreen().geometry()
        size = self.geometry()
        # move 方法：这个方法用于设置小部件的左上角的坐标位置，它需要两个参数，即横坐标和纵坐标。
        # 使用 move 方法会改变小部件的位置，但不会改变其大小。
        # 例如，widget.move(100, 100) 会将小部件的左上角移动到坐标 (100, 100)。
        self.move((screen.width() - size.width()) / 2, (screen.height() - size.height()) / 2)

    def show_geometry(self):
        print('主窗口坐标信息，相对于屏幕：')
        print('主窗口：x={}, y={},width={},heigh={}'.format(
            self.x(), self.y(), self.width(), self.height()))
        print('主窗口： geometry：x={}, y={},width={},heigh={}'.format(
            self.geometry().x(), self.geometry().y(), self.geometry().width(), self.geometry().height()))
        print('主窗口： frameGeometry：x={}, y={},width={},heigh={}'.format(
            self.frameGeometry().x(), self.frameGeometry().y(), self.frameGeometry().width(),
            self.frameGeometry().height()))

        print('\n子窗口QWidget 坐标信息，相对于主窗口：')
        print('子窗口：x={}, y={},width={},heigh={}'.format(
            self.widget.x(), self.widget.y(), self.widget.width(), self.widget.height()))
        print('子窗口： geometry：x={}, y={},width={},heigh={}'.format(
            self.widget.geometry().x(), self.widget.geometry().y(), self.widget.geometry().width(),
            self.widget.geometry().height()))
        print('子窗口： frameGeometry：x={}, y={},width={},heigh={}'.format(
            self.widget.frameGeometry().x(), self.widget.frameGeometry().y(), self.widget.frameGeometry().width(),
            self.widget.frameGeometry().height()))


if __name__ == '__main__':
    app = QApplication(sys.argv)
    myWin = MainWidget()
    myWin.show()
    sys.exit(app.exec())
```

#### 界面

![image-20240331185857895](imge/PySide6.assets/image-20240331185857895.png)

### 5.1.3 移动主窗口

```python
# 主窗口居中显示
self.button2 = QPushButton('主窗口居中')
self.button2.clicked.connect(self.center)
layout.addWidget(self.button2)

def center(self):
    # 获取的屏幕大小是被缩放过的大小，比如我电脑125%的缩放，
    # 1920*1080的屏幕大小，在程序中获得的屏幕大小是1536*864，
    # 需要还原的话直接乘个缩放倍数1.25就行。
    screen = QGuiApplication.primaryScreen().geometry()
    # 获取窗口的位置及大小
    size = self.geometry()
    # move 方法：这个方法用于设置小部件的左上角的坐标位置，它需要两个参数，即横坐标和纵坐标。
    # 使用 move 方法会改变小部件的位置，但不会改变其大小。
    # 例如，widget.move(100, 100) 会将小部件的左上角移动到坐标 (100, 100)。
    self.move((screen.width() - size.width()) / 2, (screen.height() - size.height()) / 2)
```

介绍：

```python
QGuiApplication.primaryScreen().geometry()
```

用来计算显示屏幕大大小，返回的是一个QRect类。

QRect(int left, int top, int width, int height) ：

- left、top 分别表示距离左侧和顶部的距离
- width、height 分别表示 屏幕（窗口）的宽度和高度

可以通过 QRect对象的 left()、top()、width()、 height() 函数获取值

### 5.1.4 添加图标

```python
# 显示图标
self.button3 = QPushButton('显示图标')
self.button3.clicked.connect(lambda: self.setWindowIcon(QIcon("../images/cartoon1.ico")))
self.button3.clicked.connect(lambda: self.button3.setIcon(QIcon("../images/cartoon2.ico")))
layout.addWidget(self.button3)
```

分别使用setWindowIcon()、setIcon()对窗口、控件设置图标，但该方法需要一个QIcon类型的对象作为参数。在调用QIcon对象构造函数时，需要提供图标路径（相对路径或绝对路径）。

![image-20240331191714252](imge/PySide6.assets/image-20240331191714252.png)

### 5.15 显示状态栏

```python
# 显示状态栏
self.button4 = QPushButton('显示状态栏')
self.button4.clicked.connect(
    lambda: self.status.showMessage("这是我的状态栏提示，5秒钟后消失", 5000))
layout.addWidget(self.button4)
```

![image-20240331191828484](imge/PySide6.assets/image-20240331191828484.png)

### 5.1.6 窗口坐标系统

```python
# 显示窗口坐标和大小
self.button5 = QPushButton('显示窗口坐标及大小')
self.button5.clicked.connect(self.show_geometry)
layout.addWidget(self.button5)

def show_geometry(self):
    print('主窗口坐标信息，相对于屏幕：')
    print('主窗口：x={}, y={},width={},heigh={}'.format(
        self.x(), self.y(), self.width(), self.height()))
    print('主窗口： geometry：x={}, y={},width={},heigh={}'.format(
        self.geometry().x(), self.geometry().y(), self.geometry().width(), self.geometry().height()))
    print('主窗口： frameGeometry：x={}, y={},width={},heigh={}'.format(
        self.frameGeometry().x(), self.frameGeometry().y(), self.frameGeometry().width(),
        self.frameGeometry().height()))

    print('\n子窗口QWidget 坐标信息，相对于主窗口：')
    print('子窗口：x={}, y={},width={},heigh={}'.format(
        self.widget.x(), self.widget.y(), self.widget.width(), self.widget.height()))
    print('子窗口： geometry：x={}, y={},width={},heigh={}'.format(
        self.widget.geometry().x(), self.widget.geometry().y(), self.widget.geometry().width(),
        self.widget.geometry().height()))
    print('子窗口： frameGeometry：x={}, y={},width={},heigh={}'.format(
        self.widget.frameGeometry().x(), self.widget.frameGeometry().y(), self.widget.frameGeometry().width(),
        self.widget.frameGeometry().height()))
```



PySide 6 使用统一坐标系统来定位窗口的位置和大小：

![image-20240331192514876](imge/PySide6.assets/image-20240331192514876.png)

#### pos、geometry、frameGeometry函数区别：

主窗口相对于屏幕，起点为屏幕左上角

![image-20240331192632852](imge/PySide6.assets/image-20240331192632852.png)

- X、Y、width、height：X、Y 不包含边框和标题栏，width、height 不包含边框和标题栏
- pos：X、Y 不包含边框和标题栏,没有width、height
- geometry：X、Y 包含边框和标题栏，width、height 不包含边框和标题栏
- frameGeometry：X、Y 不包含边框和标题栏,width、height 包含边框和标题栏

子窗口相对于父窗口，起点为父窗口的左上角，没有边框和标题栏 pos、geometry、frameGeometry 值都一样

#### **其它坐标相关函数：**

```
QWidget.pos()  # x 和 y 的组合，返回 QtCore.QPoint(x, y)
# width 和height 的组合，返回QtCore.QSize(width,height)，
# QMainWindow/QWidget/QDialog 都可以调用 QWidget.size()
*.size()  
# 相当于 QWidget.frameGeometry().size()
QWidget.geometry().size()，QWidget.frameGeometry().size()，QWidget.framesize()
```

#### **设置位置和尺寸：**

```
move(x, y)  # 操控的是x和y
resize(width, height)  # 操控的是宽和高，不包括窗口边框。如果小于最小值，就无效
setGeometry(x_noFrame, y_noFrame, width, height)  # 注意，此处参照为用户区域

# 在show 之后设置
adjustSize()  # 根据内容自适应大小。单次有效，在设置内容后面使用
setFixedSize()  # 设置固定尺寸
```

#### **设置最大尺寸和最小尺寸：**

```
minimumWidth()		# 返回最小尺寸的宽度
minimumHeight()		# 返回最小尺寸的高度
minimumSize()		# 返回最小尺寸

maximumWidth()		# 返回最大尺寸的宽度
maximumHeight()		# 返回最大尺寸的高度
maximumSize()		# 返回最大尺寸

setMaximumWidth()	# 设置最大宽度
setMaximumHeight()	# 设置最大高度
setMaximumSize()	# 设置最大尺寸

setMinimumWidth()	# 设置最小宽度
setMinimumHeight()	# 设置最小高度
setMinimumSize()	# 设置最小尺寸
```

## 5.2 标签（QLabel）

QLabel对象作为一个占位符可以显示不可编辑的文本和图片，也可以放置一个GIF动画，还可以用作提示标记为其它控件。

纯文本、链接、富文本都可以在标签上显示。

QLabel是界面中的标签类，继承自QFrame。继承结构图：

![image-20240331202433450](imge/PySide6.assets/image-20240331202433450.png)

**常用函数**

![image-20240331203127504](imge/PySide6.assets/image-20240331203127504.png)

**常用信号**

![image-20240331203325985](imge/PySide6.assets/image-20240331203325985.png)

### 案例：QLabel标签的基本用法

![image-20240331225348884](imge/PySide6.assets/image-20240331225348884.png)

### 5.2.1 对齐

setAlignment() 是QLabel、QLineEdit等控件通用的函数，用来设置文本的对齐方式，[详见](#5.2 标签（QLabel） "常用函数" )

```python
# 显示普通标签
label_normal = QLabel(self)
label_normal.setText("这是一个普通标签，居中")
# 水平方向居中对齐
label_normal.setAlignment(Qt.AlignCenter)
```

### 5.2.2 设置颜色

```python
# 背景标签
label_color = QLabel(self)
label_color.setText("这是一个有红色背景白色字体的标签，左对齐。")

# 在QLabel类中使用QPalette类一定要设置，否则QPalette类无法管理颜色
label_color.setAutoFillBackground(True)
# QPalette类作为对话框或控件的调色板，管理着所有颜色信息
palette = QPalette()
# QPalette.Window 表示背景色
palette.setColor(QPalette.Window, Qt.red)
# QPalette.WindowText 前景色
palette.setColor(QPalette.WindowText, Qt.white)
label_color.setPalette(palette)

# 水平方向左对齐
label_color.setAlignment(Qt.AlignLeft)
```

### 5.2.3 显示HTML信息

QLabel 可以兼容HTML格式，使用HTML可以呈现更丰富的文字形式。

```python
# HTML标签
label_html = QLabel(self)
label_html.setText("<a href='#'>这是一个html标签</a> <font color=red>hello <b>world</b> </font>")
```

### 5.2.4 滑动与单击事件

```python
# 滑过 QLabel绑定槽事件
label_hover = QLabel(self)
label_hover.setText("<a href='#'>指针滑过该标签触发事件</a>")
label_hover.linkHovered.connect(self.link_hovered)

# 单击 QLabel 绑定事件
label_click = QLabel(self)
label_click.setText("<a href='https://www.baidu.com'>单击可以打开百度</a>")
label_click.linkActivated.connect(self.link_clicked)
# 当单击标签中嵌入的超链接，并且希望在新窗口中打开该超链接时，
# setOpenExternalLinks特性必须设置为True
label_click.setOpenExternalLinks(True)

def link_hovered(self):
    print("指针滑过该标签触发事件。")

def link_clicked(self):
    # 设置了 setOpenExternalLinks(True)之后会自动屏蔽该信号
    print("当单击 ‘单击可以打开百度’ 超链接时，触发事件。")
```

QLabel有两个常用信号，即linkHovered和linkActivated，当鼠标指针滑过超链接或单击超链接时才会触发。

==注意：==

- 只对超链接有效，即`<a href ='...'`带有href的HTML文本有效。

- 如果设置了 setOpenExternalLinks(True)，则linkActivated信号不会起作用,上面代码不会触发link_clicked信号。

- 当单击标签中嵌入的超链接，并且希望在新窗口中打开该超链接时，setOpenExternalLinks特性必须设置为True。

### 5.2.5 加载图片和气泡提示QToolTip

```python
# 显示图片
label_pic = QLabel(self)
label_pic.setAlignment(Qt.AlignCenter)
label_pic.setToolTip('这是一个图片标签')
label_pic.setPixmap(QPixmap("images/cartoon1.ico"))
```

QLabel类： 

- 使用 setPixmap()函数加载图片信息
- 使用setToolTip()函数进行气泡提示，函数是QWidget的函数，任何继承QWidget类控件都可以使用。

![image-20240331234642690](imge/PySide6.assets/image-20240331234642690.png)

### 5.2.6 使用快捷键

#### 案例：QLabel快捷键的基本用法

QLabel快捷键本身没有太大意义，但可以通过伙伴关系（setBuddy）把指针快速切换到目标控件（如文本框）上，以便于使用快捷键操作。

![image-20240331235258639](imge/PySide6.assets/image-20240331235258639.png)

```python
import sys

from PyQt6.QtWidgets import QApplication
from PySide6.QtWidgets import QDialog, QLabel, QLineEdit, QPushButton, QGridLayout


class QLabelDemo(QDialog):
    def __init__(self):
        super().__init__()
        self.setWindowTitle('QLabel 例子')
        nameLb1 = QLabel('&Name', self)
        nameEb1 = QLineEdit(self)
        nameLb1.setBuddy(nameEb1)

        nameLb2 = QLabel('&Password', self)
        nameEb2 = QLineEdit(self)
        nameLb2.setBuddy(nameEb2)

        btnOk = QPushButton('&OK')
        btnCancel = QPushButton('&Cancel')

        mainLayout = QGridLayout(self)
        mainLayout.addWidget(nameLb1, 0, 0)
        mainLayout.addWidget(nameEb1, 0, 1, 1, 2)

        mainLayout.addWidget(nameLb2, 1, 0)
        mainLayout.addWidget(nameEb2, 1, 1, 1, 2)

        mainLayout.addWidget(btnOk, 2, 1)
        mainLayout.addWidget(btnCancel, 2, 2)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    labelDemo = QLabelDemo()
    labelDemo.show()
    sys.exit(app.exec())
```

## 5.3 单行文本框（QLineEdit）

QLineEdit类是一个单行文本框控件，可以输入单行字符串。如果需要输入多行字符串，则使用QTextEdit类。

QLabel通过QFrame继承QWidget，而QLineEdit直接继承QWidget，继承结构图：

![image-20240401085124583](imge/PySide6.assets/image-20240401085124583.png)

### **常用函数:**

| 函数                        | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| setAlignment()              | 按固定值方式对齐文本。<br>- Qt.AlignLeft：水平方向靠左对齐<br>- Qt.AlignRight：水平方向靠右对齐<br>- Qt.AlignCenter：水平方向居中对齐<br>- Qt.AlignJustify：水平方向调整间距两端对齐<br>- Qt.AlignTop：垂直方向靠上对齐<br>- Qt.AlignBottom：垂直方向靠下对齐<br>- Qt.AlignVCenter：垂直方向居中对齐 |
| clear()                     | 清除文本框中的内容                                           |
| backspace()                 | 删除指针左侧的字符或选中的文本                               |
| del_()                      | 删除指针右侧的字符或选中的文本                               |
| copy()                      | 复制文本框中的内容                                           |
| cut()                       | 剪切文本框中的内容                                           |
| paste()                     | 粘贴文本框内容                                               |
| isUndoAvailabQLineEdit()    | 是否可以执行撤销动作                                         |
| undo()                      | 撤销                                                         |
| redo()                      | 重做                                                         |
| setDragEnabQLineEditd(True) | 设置文本可拖拽                                               |
| setEchoMode()               | 设置文本框的显示格式。允许输入的文本框显示格式的值可以是:<br>- QLineEdit.Normal：正常显示输入的字符，此为默认选项<br>- QLineEdit.NoEcho：不显示任何输入的字符，常用于密码类型的输入，并且密码长度需要保密<br>- QLineEdit.Password：显示与平台相关的密码掩码字符，Windows 常用的是星号<br>- QLineEdit.PasswordEchoOnEdit：在编辑时短暂显示该字符，然后迅速将该字符显示为掩码字符 |
| setPlaceholderText()        | 设置文本框浮显文字                                           |
| setMaxLength()              | 设置文本框允许输入的最大字符数，默认情况下，单行编辑控件的文本长度限制为 32767，获取单行编辑控件的文本长度限定的函数为：maxLength() |
| setReadOnly(True)           | 设置文本框是只读的                                           |
| setText()                   | 设置文本框中的内容                                           |
| text()                      | 返回文本框中的内容                                           |
| setDragEnabled()            | 设置文本框是否接受拖动                                       |
| selectAll()                 | 全选                                                         |
| setFocus()                  | 得到焦点                                                     |
| setInputMask()              | 设置掩码                                                     |
| setValidator()              | 设置文本框的验证器（验证规则），将限制任意可能输入的文本。可用校验器如下：<br>- QIntValidator：限制输入整数<br>- QDoubleValidator：限制输入浮点数<br>- QRegexpValidator：检查输入是否符合正则表达式 |

### **指针操作函数**

| 函数                               | 描述                                             |
| ---------------------------------- | ------------------------------------------------ |
| cursorBackward(mark=True, steps=0) | 向左移动step个字符，当mark为True时带选中效果     |
| cursorForward(mark=True, steps=0)  | 向右移动step个字符，当mark为True时带选中效果     |
| cursorWordBackward(mark=True)      | 向左移动一个单词的长度，当mark为True时带选中效果 |
| cursorWordForward(mark=True)       | 向右移动一个单词的长度，当mark为True时带选中效果 |
| home(mark=True)                    | 指针移到行首，当mark为True时带选中效果           |
| end(mark=True)                     | 指针移到行尾，当mark为True时带选中效果           |
| setCursorPosition(pos=8)           | 指针移到指定位置（如果pos为小数则向下取整）      |
| cursorPosition()                   | 获取指针的位置                                   |
| setFocus()                         | 获取输入焦点                                     |
| hasFocus()                         | 查询是否获取输入焦点，返回 bool 类型             |



### **常用信号：**

| 信号                                          | 说明                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| textChanged                                   | 当修改文本内容时，这个信号会被发射                           |
| textEdited(text)                              | 当文本被编辑时，就会发射这个信号，通过setText()函数更改文本，不会触发此信号 |
| returnPressed                                 | 光标在行编辑框内时，点击**回车键**即发射信号，**注意：**编辑行设置了validator()或inputMask()，只有当输入在inputMask()之后，并且validator()返回QValidator.Acceptable时，才会发射。 |
| selectionChanged                              | 当选择的文本内容改变了，这个信号就会被发射                   |
| editingFinished                               | 当按返回或者**回车键**时，或者行编辑失去焦点时，这个信号会被发射 |
| cursorPositionChanged(int oldPos, int newPos) | 当焦点，即**光标**位置改变就发射信号,前一个位置由oldPos给出，新位置由newPos给出 |
| inputRejected                                 | 当用户输入不合法字符时，将发出此信号。前提要 setValidator() 等设置合法字符范围，**Qt 5.12 版本新增**。 |

### 案例：QLineEdit的基本用法

QLineEdit类的很多函数和QLabel类的函数一样，如对齐、颜色设置、tooltip设置等。但二者也存在不同之处，如QLineEdit类不支持HTML显示，没有滑动信号和单击信号。

![image-20240401153147712](imge/PySide6.assets/image-20240401153147712.png)

### 5.3.1 对齐、tooltip和颜色设置

对齐、tooltip、颜色设置与QLabel类一致，设置颜色有差异：

- 背景色：QLabel类 QPalette.Window ，QLineEdit类 QPalette.Base
- 前景色时，QLabel类 QPalette.WindowText，QLineEdit类 QPalette.Text

```python
# 正常文本框,对齐，tooltip
lineEdit_normal = QLineEdit()
# 设置内容
lineEdit_normal.setText("122")
# 居中
lineEdit_normal.setAlignment(Qt.AlignCenter)
# 气泡提示
lineEdit_normal.setToolTip("这是一个普通文本框")
flo.addRow("普通文本框，居中", lineEdit_normal)

# 显示颜色
lineEdit_color = QLineEdit()
# 设置内容
lineEdit_color.setText("显示红色背景白色字体")
# 设置颜色
lineEdit_color.setAutoFillBackground(True)
palette = QPalette()
palette.setColor(QPalette.Base, Qt.red)
palette.setColor(QPalette.Text, Qt.white)
lineEdit_color.setPalette(palette)
# 左对齐
lineEdit_color.setAlignment(Qt.AlignLeft)
flo.addRow("显示颜色，左对齐", lineEdit_color)
```

### 5.3.2 占位提示符、限制输入长度、限制编辑

- 占位提示符：文本框中有提示文本，当输入内容后提示文本自动消失并被输入的值替代，使用setPlaceholderText设置
- 限制输入长度：使用setMaxLength设置
- 限制编辑：使用setReadOnly(True)设置

```python
# 占位提示符，限制长度
lineEdit_maxLength = QLineEdit()
# 设置文本框浮显文字
lineEdit_maxLength.setPlaceholderText("最多输入5个字符")
lineEdit_maxLength.setMaxLength(5)
flo.addRow("最多输入5个字符", lineEdit_maxLength)

# 只读文本
lineEdit_readOnly = QLineEdit()
lineEdit_readOnly.setReadOnly(True)
lineEdit_readOnly.setText("只读文本，不能编辑")
flo.addRow("只读文本", lineEdit_readOnly)
```

### 5.3.3 移动指针

移动指针涉及焦点：

- setFocus表示获取焦点
- hasFocus表示是否获取到焦点
- setCursorPosition(1)表示设置文本框当前指针的位置为1
- cursorForward(bool mask, int steps=1)表示向前（向右）移动指针，mark为True 选中移动的字符，steps 移动字符数量

[相关函数参见](# 指针操作函数 "指针函数")

```python
# 移动光标
lineEdit_cursor = QLineEdit()
lineEdit_cursor.setText("单击左边按钮向右移动光标")
lineEdit_cursor.setFocus()
lineEdit_cursor.setCursorPosition(1)
button = QPushButton("点我右移光标")
self.lineEdit_cursor = lineEdit_cursor
button.clicked.connect(self.move_cursor)
flo.addRow(button, lineEdit_cursor)

def move_cursor(self):
    # cursorForward(mark = True, steps = 0)
    # 向右移动step个字符，当mark为True时带选中效果
    self.lineEdit_cursor.cursorForward(True, 2)
```

### 5.3.4 编辑

删除文本 按钮触发槽函数QLineEdit.clear()把文本框所有内容删除

```python
# 编辑文本
lineEdit_edit = QLineEdit()
lineEdit_edit.setText("编辑文本")
button2 = QPushButton("删除文本")
button2.clicked.connect(lambda: lineEdit_edit.clear())
flo.addRow(button2, lineEdit_edit)
```

### 5.3.5 相关信号与槽

使用textChanged信号，当修改文本内容时，就会触发槽函数，修改标签的结果。

[参见](#”常用信号：" "信号")

```python
# 槽函数
lineEdit_change = QLineEdit()
lineEdit_change.setPlaceholderText("输入文本框会改变左侧标签")
lineEdit_change.setFixedWidth(200)
label = QLabel("槽函数应用")
lineEdit_change.textChanged.connect(
    lambda: label.setText("更新标签：" + lineEdit_change.text()))
flo.addRow(label, lineEdit_change)
```

### 5.3.6 快捷键

默认快捷键如下表，此外，还提供了一个上下文菜单（在单击鼠标右键时调用），其中显示了一些编辑选项。

| 快捷键           | 作用                         |
| ---------------- | ---------------------------- |
| ←                | 将指针向左移动一个字符       |
| Shift + ←        | 向左移动一个字符并选择文本   |
| →                | 将指针向右移动一个字符       |
| Shift + →        | 向右移动一个字符并选择文本   |
| Home             | 将指针移到行首               |
| End              | 将指针移到行尾               |
| Backspace        | 删除指针左侧的字符           |
| Ctrl + Backspace | 删除指针左侧的单词           |
| Delete           | 删除指针右侧的字符           |
| Ctrl + Delete    | 删除指针右侧的单词           |
| Ctrl + A         | 全选                         |
| Ctrl + C         | 将选定的文本复制到剪贴板中   |
| Ctrl + Insert    | 将选定的文本复制到剪贴板中   |
| Ctrl + K         | 删除到行尾                   |
| Ctrl + V         | 将剪贴板文本粘贴到行编辑器中 |
| Shift + Insert   | 将剪贴板文本粘贴到行编辑器中 |
| Ctrl + X         | 剪贴所选中文本               |
| Shift + Delete   | 剪贴所选中文本               |
| Ctrl + Z         | 撤销上次的操作               |
| Ctrl + Y         | 重做上次撤销的操作           |

![image-20240401172317100](imge/PySide6.assets/image-20240401172317100.png "上下文菜单")

### 5.3.7 隐私保护：回显模式

在网页中输入密码之后会显示 `*`，对用户隐私进行保护，在PySide中通过回显模式（EchoMode）来设置。

[具体设置参见](#常用函数:  "setEchoMode()")

案例：回显模式的显示效果

```python
# -*- coding: utf-8 -*-

'''
    【简介】
	PySide6中 QLineEdit.EchoMode效果例子
  
'''

from PySide6.QtWidgets import QApplication, QLineEdit, QWidget, QFormLayout
import sys


class lineEditDemo(QWidget):
    def __init__(self, parent=None):
        super(lineEditDemo, self).__init__(parent)
        self.setWindowTitle("QLineEdit_EchoMode例子")

        flo = QFormLayout()
        pNormalLineEdit = QLineEdit()
        pNoEchoLineEdit = QLineEdit()
        pPasswordLineEdit = QLineEdit()
        pPasswordEchoOnEditLineEdit = QLineEdit()

        flo.addRow("Normal", pNormalLineEdit)
        flo.addRow("NoEcho", pNoEchoLineEdit)
        flo.addRow("Password", pPasswordLineEdit)
        flo.addRow("PasswordEchoOnEdit", pPasswordEchoOnEditLineEdit)

        pNormalLineEdit.setPlaceholderText("Normal")
        pNoEchoLineEdit.setPlaceholderText("NoEcho")
        pPasswordLineEdit.setPlaceholderText("Password")
        pPasswordEchoOnEditLineEdit.setPlaceholderText("PasswordEchoOnEdit")

        # 设置显示效果
        pNormalLineEdit.setEchoMode(QLineEdit.Normal)
        pNoEchoLineEdit.setEchoMode(QLineEdit.NoEcho)
        pPasswordLineEdit.setEchoMode(QLineEdit.Password)
        pPasswordEchoOnEditLineEdit.setEchoMode(QLineEdit.PasswordEchoOnEdit)

        self.setLayout(flo)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = lineEditDemo()
    win.show()
    sys.exit(app.exec())
```

效果：

![image-20240401181410192](imge/PySide6.assets/image-20240401181410192.png)

### 5.3.8 限制输入：验证器

在通常情况下，需要对用户的输入做一些限制，如只允许输入整数、浮点数或其它自定义数据，验证器（QValidator）可以满足这些限制需求。验证器由 QValidator 控制。

setValidator()  

设置文本框的验证器（验证规则），将限制任意可能输入的文本。可用校验器如下：

- QIntValidator：限制输入整数
- QDoubleValidator：限制输入浮点数
- QRegexpValidator：检查输入是否符合正则表达式

案例：QValldator验证器的使用方法

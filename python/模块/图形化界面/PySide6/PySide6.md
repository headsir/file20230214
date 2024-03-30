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

同 转换ui文件类似，使用 pyside6-rcc.exe

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

![image-20240330080810541](imge/PySide6.assets/image-20240330080810541.png)

布局一般有两种方式：

- 使用布局管理器
- 使用容器控件
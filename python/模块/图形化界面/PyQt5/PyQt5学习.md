PyQt5学习 2022.12.6

# 一、安装模块

```python
pip install pyqt5
```

<font color = red size =5>注意事项：PyQt5安装路径不能包含中文字符</font>

# 二、业务与逻辑分离实现

信号与槽函数的使用

```python
"""
  这一步主要实现业务逻辑，也就是点击登录和退出按钮后程序要执行的操作。为了后续维护方便，采用界面与业务逻辑相分离来实现。
  也就是通过创建主程序调用界面文件方式实现。这有2个好处。第1就是实现逻辑清晰。
  第2就是后续如果界面或者逻辑需要变更，好维护。新建py文件程序，调用登录窗体练习.py文件。
"""
import sys  # 导入系统模块
from PyQt5.QtWidgets import QApplication, QMainWindow  # 导入pyQT5模块
from QT5学习.登录窗体练习 import Ui_Form  # 导入窗体模块


class MyMainForm(QMainWindow, Ui_Form):  # 类继承父类QMainWindow,Ui_Form
    def __init__(self, parent=None):  # 类初始化
        # super() 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，
        # 但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。
        super(MyMainForm, self).__init__(parent)
        self.setupUi(self)  # 调用Ui_Form父类函数setupUi
        # 添加登录按钮信号和槽，注意display函数不加小括号
        self.login_pushButton.clicked.connect(self.display)
        # 添加退出按钮信号和槽，调用close函数
        self.cancel_pushButton.clicked.connect(self.close)  # close关闭函数

    def display(self):
        # 利用line Edit控件对象text()函数获取界面输入
        username = self.user_lineEdit.text()
        password = self.pwd_lineEdit.text()
        # 利用text Browser控件对象setText()函数设置界面显示
        self.user_textBrowser.setText('登录成功！\n' + '用户名是：' + username + '\n密码是：' + password)


if __name__ == '__main__':
    # 固定的，PyQt5程序都需要QApplication对象，sys.argv是命令行参数列表，确保程序可以双击运行
    app = QApplication(sys.argv)
    mywin = MyMainForm()  # 实类初始化
    mywin.show()  # 将窗口控件显示在屏幕上
    sys.exit(app.exec_())  # 程序运行，sys.exit方法确保程序完整退出
```

# 三、Ui文件加载方式

## 动态加载Ui文件

```python
from PyQt5 import uic

class Chengxu(QMainWindow):
    def __init__(self, pandas=None):
        super(Chengxu, self).__init__(pandas)
        # 从文件中加载UI定义
        uic.loadUi('日常小程序.ui', self)
        self.Flie_nameh_Button.clicked.connect(self.Flie_nameh)

白月黑羽建议：通常采用动态加载比较方便，因为改动界面后，不需要转化，直接运行，特别方便。

但是，如果 你的程序里面有非qt designer提供的控件， 这时候，需要在代码里面加上一些额外的声明，而且 可能还会有奇怪的问题。往往就 要采用 转化Python代码的方法
```

## 静态加载Ui文件

```
from QT5学习.登录窗体练习 import Ui_Form  # 导入窗体模块


class MyMainForm(QMainWindow, Ui_Form):  # 类继承父类QMainWindow,Ui_Form
    def __init__(self, parent=None):  # 类初始化
        # super() 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，
        # 但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。
        super(MyMainForm, self).__init__(parent)
        self.setupUi(self)  # 调用Ui_Form父类函数setupUi
        # 添加登录按钮信号和槽，注意display函数不加小括号
        self.login_pushButton.clicked.connect(self.display)
        # 添加退出按钮信号和槽，调用close函数
        self.cancel_pushButton.clicked.connect(self.close)  # close关闭函数
```

![image-20220320225529961](imge/加载Ui文件.assets/image-20220320225529961.png)

# 四、案例学习

## 4.1 创建空白窗体

```python
"""
1、安装模块    pip install pyqt5
2、super() 函数说明：
    用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，
    但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。
"""
import sys  # 导入系统模块

# 导入pyQT5模块
from PyQt5.QtWidgets import QApplication, QWidget, QDesktopWidget

class MainWindow(QWidget):  # 类继承父类QWidget
    def __init__(self):  # 类初始化
        super().__init__()
        # 窗体标题和尺寸
        self.setWindowTitle("NB的xx系统")
        # 窗体的尺寸
        self.resize(980, 450)

        # 窗体位置
        qr = self.frameGeometry()
        cp = QDesktopWidget().availableGeometry().center()
        qr.moveCenter(cp)

if __name__ == '__main__':
    # 固定的，PyQt5程序都需要QApplication对象，sys.argv是命令行参数列表，确保程序可以双击运行
    app = QApplication(sys.argv)
    mywin = MainWindow()  # 实类初始化
    mywin.show()  # 将窗口控件显示在屏幕上
    sys.exit(app.exec_())  # 程序运行，sys.exit方法确保程序完整退出
```

## 4.2 窗口布局

```python
"""
1、安装模块    pip install pyqt5
2、super() 函数说明：
    用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，
    但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。
3、布局模块
    QHBoxLayout 水平布局
    QVBoxLayout 垂直布局
4、 功能模块
    QPushButton 按钮
    QLineEdit 输入框
    QTableWidget 表格
    QTableWidgetItem 单元格对象
    QLabel 标签
    QT 属性设置
"""
import sys  # 导入系统模块

# 导入pyQT5模块
from PyQt5.QtWidgets import QApplication, QWidget, QDesktopWidget
from PyQt5.QtWidgets import QHBoxLayout, QVBoxLayout
from PyQt5.QtWidgets import QPushButton, QLineEdit, QTableWidget, QTableWidgetItem, QLabel
from PyQt5.QtCore import Qt

class MainWindow(QWidget):  # 类继承父类QWidget
    def __init__(self):  # 类初始化
        super().__init__()
        # 窗体标题和尺寸
        self.setWindowTitle("NB的xx系统")
        # 窗体的尺寸
        self.resize(1228, 450)

        # 窗体位置
        qr = self.frameGeometry()
        cp = QDesktopWidget().availableGeometry().center()
        qr.moveCenter(cp)

        # 创建垂直布局，一级布局
        layout = QVBoxLayout()

        layout.addLayout(self.init_header())
        layout.addLayout(self.init_form())
        layout.addLayout(self.init_table())
        layout.addLayout(self.init_footer())

        # 给窗体设置元素的排列方式
        self.setLayout(layout)

        # 弹簧
        # layout.addStretch()
        
    def init_header(self):
        # 1.创建顶部菜单布局，二级布局
        header_layout = QHBoxLayout()  # 水平布局
        # 1.1 创建按钮，加入header_layout
        btn_start = QPushButton("开始")
        # # 设置按钮高度、宽度
        # btn_start.setFixedHeight(100)
        # btn_start.setFixedWidth(200)
        header_layout.addWidget(btn_start)

        btn_stop = QPushButton("停止")
        header_layout.addWidget(btn_stop)

        # 弹簧
        header_layout.addStretch()
        return header_layout
    
    def init_form(self):
        # 2.创建标题布局，二级布局
        form_layout = QHBoxLayout()

        # 2.1 输入框
        txt_asin = QLineEdit()
        txt_asin.setPlaceholderText("请输入商品ID和价格")  # 默认值设置
        form_layout.addWidget(txt_asin)
        # 2.2 添加按钮
        btn_add = QPushButton("添加")
        form_layout.addWidget(btn_add)
        return form_layout
    
    def init_table(self):
        # 3.创建中间的表格
        table_layout = QHBoxLayout()

        # 3.1 创建表格
        table_widget = QTableWidget(0, 8)  # 表格0行8列
        table_layout.addWidget(table_widget)

        table_header = [
            {"field": "asin", "text": "ASIN", "width": 120},
            {"field": "title", "text": "标题", "width": 150},
            {"field": "url", "text": "URL", "width": 400},
            {"field": "price", "text": "低价", "width": 100},
            {"field": "success", "text": "成功次数", "width": 100},
            {"field": "error", "text": "503次数", "width": 100},
            {"field": "status", "text": "状态", "width": 100},
            {"field": "frequency", "text": "频率（N秒/次）", "width": 120}
        ]

        for idx, info in enumerate(table_header):
            item = QTableWidgetItem()
            item.setText(info["text"])
            table_widget.setHorizontalHeaderItem(idx, item)
            table_widget.setColumnWidth(idx, info["width"])  # 设置列宽
        return table_layout
    
    def init_footer(self):
        # 4.创建底部菜单
        footer_layout = QHBoxLayout()

        label_status = QLabel("未检测", self)
        footer_layout.addWidget(label_status)

        # 弹簧
        footer_layout.addStretch()

        btn_reinit = QPushButton("重新初始化")
        footer_layout.addWidget(btn_reinit)

        btn_recheck = QPushButton("重新检测")
        footer_layout.addWidget(btn_recheck)

        btn_reset_count = QPushButton("次数清零")
        footer_layout.addWidget(btn_reset_count)

        btn_delete = QPushButton("删除检测项")
        footer_layout.addWidget(btn_delete)

        btn_alert = QPushButton("SMTP报警配置")
        footer_layout.addWidget(btn_alert)

        btn_proxy = QPushButton("代理IP")
        footer_layout.addWidget(btn_proxy)
        return footer_layout

if __name__ == '__main__':
    # 固定的，PyQt5程序都需要QApplication对象，sys.argv是命令行参数列表，确保程序可以双击运行
    app = QApplication(sys.argv)
    mywin = MainWindow()  # 实类初始化
    mywin.show()  # 将窗口控件显示在屏幕上
    sys.exit(app.exec_())  # 程序运行，sys.exit方法确保程序完整退出
```

![image-20221206145040199](imge/PyQt5学习.assets/image-20221206145040199.png)

## 4.3 表格数据初始化

- 有数据

- 表格中展示

  ```python
  # 3.2 初始化表格数据
  # 读取数据文件
  import json
  file_path = "db.json"
  with open(file_path, mode='r', encoding='utf-8') as f:
      data = f.read()
      data_list = json.loads(data)['RECORDS']  # 根据文件设置
  
      current_roe_count = table_widget.rowCount()  # 当前表格有多少行
      for row_list in data_list:
          table_widget.insertRow(current_roe_count)  # 添加行
  
          # 写数据
          # cell = QTableWidgetItem(row_list["日期"])  # 获取内容
          # table_widget.setItem(current_roe_count, 0, cell)  # 参数：行、列、内容
          # current_roe_count += 1
          for i, ele in enumerate(row_list):
              cell = QTableWidgetItem(row_list[str(ele)])  # 获取内容
              if i in [0, 4, 6, 7]:
                  # 设置不可修改
                  cell.setFlags(Qt.ItemIsSelectable | Qt.ItemIsEnabled)
                  table_widget.setItem(current_roe_count, i, cell)  # 参数：行、列、内容
                  current_roe_count += 1
  ```

  

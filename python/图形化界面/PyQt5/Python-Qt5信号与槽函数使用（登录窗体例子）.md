# 例子：登录窗体

```
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
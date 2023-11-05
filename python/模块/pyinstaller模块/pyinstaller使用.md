# pyinstaller打包程序

# 安装pyinstaller

```
pip install pyinstaller
```

# 准备图标

图标使用48*48大小(可以按需调整)的ico图标比较合适，可以在线转换

## 语法：

pyinstaller 参数1 参数2 参数3 参数4 -------参数n  xxx.py

如果需要查看pyinstaller的所有参数，可以在命令行执行pyinstaller --help查看。

```
pyinstaller --help
```



参数含义：

| 参数    | 具体参数               | 含义                                                         | 备注     |
| ------- | ---------------------- | ------------------------------------------------------------ | -------- |
| -F      | --onefile              | 产生单个的可执行文件（创建一个.exe可执行文件，所有的第三方依赖和代码均被打包进该.exe文件） |          |
| -D      | --onedir               | 产生一个文件夹，其中包含可执行程序文件                       |          |
| -w      | --windowed,--noconsole | 程序运行时不显示命令行窗口（仅对Windows有效）                |          |
| -c      | --nowindowed,--console | 程序运行时显示命令行窗口（仅对Windows有效）                  |          |
| -o DIR  | --out=DIR              | 指定spec文件的生成文件夹，如果没有指定，<br>则默认使用当前文件夹来生成spec文件 | 使用失败 |
| -p DIR  | --path=DIR             | 设置Python导入模块的路径（和设置PYTHONPATH环境变量的作用相似<br>也可使用路径分隔符（Windows使用分号，Linux使用冒号）来分隔多个路径 |          |
| -n NAME | --name=NAME            | 指定项目（生成的spec文件）的名字。如果省略该选项，<br>则使用第一个Python代码文件的文件主名作为spec文件的名字 |          |
| -i FILE | --icon=FILE            | 指定可执行程序的文件图标                                     |          |

打包结果

- __pycache__: 存放了Python程序的二进制.pyc文件，.pyc文件是Python程序(后缀.py)编译出来的二进制文件，能够提高加载速度，以及隐藏源代码。
- build: 存放打包过程中生成的临时文件。
- dist: 存放打包后的.exe可执行文件。
- .spec : 打包的配置文件(可以从这个文件中对打包的程序进行相关配置，不过一般不需要) 。



`pyinstaller -F<单个可执行程序文件> -n chengxu<exe程序名> name.py<需要打包的py文件> -i icon.icon<exe图标> `

```
pyinstaller -F -n chengxu main.py -w
```



图标使用介绍

```python
我们程序运行的窗口，需要显示自己的图标，这样才更像一个正式的产品。

通过如下代码，我们可以把一个png图片文件作为 程序窗口图标。

from PySide2.QtGui import  QIcon

app = QApplication([])
# 加载 icon
app.setWindowIcon(QIcon('logo.png'))

注意：这些图标png文件，在使用PyInstaller创建可执行程序时，也要拷贝到程序所在目录。否则可执行程序运行后不会显示图标。
应用程序图标

应用程序图标是放在可执行程序里面的资源。

可以在PyInstaller创建可执行程序时，通过参数 --icon="logo.ico" 指定。

比如

pyinstaller httpclient.py --noconsole --hidden-import PySide2.QtXml --icon="logo.ico"

注意参数一定是存在的ico文件，不能是png等图片文件。

如果你只有png文件，可以通过在线的png转ico文件网站，生成ico

注意：这些应用程序图标ico文件，在使用PyInstaller创建可执行程序时，不需要要拷贝到程序所在目录。因为它已经被嵌入可执行程序了。
```




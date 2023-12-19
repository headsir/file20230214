# 一、Pyside2模块安装

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

​		PySide（在本文中代指PySide2和PySide6）是一个Python的图形化界面（GUI）库，由C++版的Qt开发而来，在用法上基本与C++版没有特别大的差异。相对于其他Python  GUI库来说，PySide开发较快，功能更完善，而且文档支持更好。我这里就给大家简单写一个极简的快速入门教程，旨在帮助大家快速上手并使用这个库。

​		首先说一下PySide和PyQt（在本文中代指PyQt5和PyQt6）的关系，前者是Qt公司的产品，后者是第三方公司的产品，二者用法基本相同，不过在使用协议上却有很大差别，PySide可以在LGPL协议下使用，PyQt则在GPL协议下使用，这两个协议的区别，大家可以自行搜索，为了避免潜在的问题，我这里推荐使用PySide。

​		现在说一下PySide2和PySide6的区别，也就是PyQt5和PyQt6的区别。PySide2和PyQt5由C++版的Qt5开发而来.，而PySide6和PyQt6对应的则是C++版的Qt6。从PySide6开始，PySide的命名也会与Qt的大版本号保持一致，不会再出现类似PySide2对应Qt5这种容易混淆的情况。

​		在使用层面上，PySide2/PyQt5和PySide6/PyQt6并无过多的差异，只有一点需要注意，使用PySide6/PyQt6开发的程序在默认情况下，不兼容Windows7系统，这也是Qt6所决定的。在下面和之后的文章中，我会尽量兼容PySide2和PySide6这两个版本，如果有存在差异的地方，我会两个版本分别解释。

​		那么开始进入正题，先介绍一下使用PySide开发GUI的基本知识（PyQt同理，下面就不再提它了）。

​		PySide为我们提供了两种开发界面的方式，一种叫QtWidget，是在网上搜到的教程中最常见的方式；另一种叫QML，是一种新型的开发方式，也是Qt正在努力推广的开发方式。在本系列的文章中，我们主要使用QtWidget这种方式，而使用QtWidget开发程序时，也有两种基本的使用方法，一种是通过designer开发界面，另一种是用过代码手动开发界面，这里我们的目的是极简快速入门，所以使用designer这种方便的方式进行开发。

### designer介绍

designer界面如下：

![image-20231219112603776](imge/Pyside2学习.assets/image-20231219112603776.png)




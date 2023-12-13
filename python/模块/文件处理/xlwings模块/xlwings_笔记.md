# **一、什么是xlwings模块**

1、xlwings是Python操作excel读写操作的第三方库

2、特色:  xlwings支持对xls和xlsx文件的读写，相对于其他第三方库xlrd,xlwd,openpyxl等效率高，可扩展性强。可以和matplotlib以及pandas无缝连接，也可以调用Excel文件中VBA写好的程序，和让VBA调用用Python写的程序。

# **二、如何使用**

## 1、安装

pip install xlwings

## 2、应用模式

对象》工作簿》工作表》表格内容

![image-20231213111424207](imge/xlwings_笔记.assets/image-20231213111424207.png)

可以简单的理解，建立一个对象，就是打开一个excel程序，一个excel应用可以管理多个工作薄，一个工作薄中可以由多张表，一个表中有多个单元格

## **3、基本操作**
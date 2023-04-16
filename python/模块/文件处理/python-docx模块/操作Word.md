# **python-docx操作word**

来源：https://www.bilibili.com/video/BV1FC4y1Y7QC?p=2

安装模块

```
pip install python-docx
```

# 基础知识

```
from docx import Document

文件 = Document("文件路径")
段落 = 文件.paragraphs  # 是一个对象列表
块 = 段落.runs  # 不同字体、颜色、大小为一个块对象
```

## 读取 word 中的文字、标题等

## 写入和插入word文字和段落

## 插入与删除图片

## 按比列设置图片

## 设置图片的对齐方式

## 表格的添加查看和定位

## 删除表的行和列、删除整表

## 单元格赋值与表格录入

## 清空单元格里的数据

## 表格与单元格对齐

## 表格样式

## 文字样式调整

## 对齐方式和行间距

## 段落缩进

## 节的添加、定位和分节符的设置

## 设置页眉和页脚与节设置

## 页眉页脚的举例

## 设置奇偶页不同

## 首页不同

## 页面边距与装订线

## 将某页设置为横向

## 页面大小和纸张方向

# 案例：

## 1、WORD段落汇总-塔租方案汇总

```python
'''
    #利用python读取word文档，先读取段落,然后保存到excel中
'''
import os
import sys

# 导入所需库
from docx import Document
import pandas as pd
path = r"D:\桌面\2023塔租压降方案\最终方案"
file_list = os.listdir(f"{path}\方案")
df_all = pd.DataFrame()
for i  in file_list:
    file_path = os.path.join(f"{path}\方案",i)
    print(f"{file_path}=====开始处理")
    # 打开word文档
    document = Document(file_path)
    # 获取所有段落
    all_paragraphs = document.paragraphs
    # 打印看看all_paragraphs是什么东西
    print(type(all_paragraphs))  # <class 'list'>，打印后发现是列表
    # 是列表就开始循环读取
    list_=[]
    for paragraph in all_paragraphs:
        # 打印每一个段落的文字
        # print(paragraph.text)
        list_.append(paragraph.text)
        print(list_)
    df = pd.DataFrame(list_)
    df_all = pd.concat([df_all,df.T])
    # df_all = pd.merge(df,df_all,left_index = True , right_index = True,how="outer")
    print(f"{file_path}=====处理结束")
df_all.to_excel(f"{path}\\汇总结果.xlsx")
```


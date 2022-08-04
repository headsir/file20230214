# win32com操作ppt

官方文档：https://docs.microsoft.com/zh-cn/office/vba/api/powerpoint.shape.copy

# 案例学习

## 一、win32com复制ppt模板

```
import win32com
from win32com.client import Dispatch
import os

ppt = Dispatch('PowerPoint.Application')
# 或者使用下面的方法，使用启动独立的进程：
# ppt = DispatchEx('PowerPoint.Application')

# 如果不声明以下属性，运行的时候会显示的打开word
ppt.Visible = 1  # 后台运行
ppt.DisplayAlerts = 0  # 不显示，不警告

# 创建新的PowerPoint文档
# pptSel = ppt.Presentations.Add() 
# 打开一个已有的PowerPoint文档
pptSel = ppt.Presentations.Open(os.getcwd() + "\\" + "2.2 win32 ppt测试.pptx")

# 复制模板页
pptSel.Slides(1).Copy()
#设置需要复制的模板页数
pageNums = 10
# 粘贴模板页
for i in range(pageNums):
    pptSel.Slides.Paste()

# pptSel.Save()  # 保存
pptSel.SaveAs(os.getcwd() + "\\" + "win32_copy模板.pptx")  # 另存为
pptSel.Close()  # 关闭 PowerPoint 文档
ppt.Quit()  # 关闭 office
```

# 封装模块

```
# coding = utf-8
# 模块及版本要求：pywin32

import win32com.client


class Win32Model(object):
    def __init__(self, file_path):
        # 调用APP
        self.pptApp = win32com.client.DispatchEx("PowerPoint.Application")
        self.pptApp.Visible = 1  # 0 后台运行 报错
        self.pptApp.DisplayAlerts = 0  # 不显示，不警告

        self.__open(file_path)  # 打开PPT文件

    # 打开文件
    def __open(self, file_path):
        try:
            self.pptSel = self.pptApp.Presentations.Open(file_path, False)
            return "文件打开成功"
        except Exception as error:
            self.pptSel = False
            print(error)
            return error

    # 复制PPT页
    def copy_slides(self, slides_page, pageNums):
        """
        复制特定PPT页
        :param slides_page: 指定PPT页
        :param pageNums: 复制数量
        :return:
        """
        try:
            # 复制模板页
            self.pptSel.Slides(slides_page).Copy()
            # 粘贴模板页
            for i in range(pageNums):
                self.pptSel.Slides.Paste()
        except Exception as error:
            print(error)
            return error

    # 另存为
    def SaveAs(self, as_file_path):
        self.pptSel.SaveAs(as_file_path)  # 另存为

    # 关闭对象Book、App
    def __del__(self):
        """
        退出程序函数
        :return:
        """
        if self.pptSel:
            # 保存
            self.pptSel.Save()
            # 关闭 PowerPoint 文档
            self.pptSel.Close()
        # 关闭 office
        self.pptApp.Quit()
        print("PPT操作完成")


if __name__ == '__main__':
    ppt = Win32Model(r"D:\数据库\pythonProject_learning\OfficeAutomation\file\22.pptx")
    ppt.copy_slides(2,5)
    ppt.SaveAs(r"D:\数据库\pythonProject_learning\OfficeAutomation\PPT\22_1.pptx")

```


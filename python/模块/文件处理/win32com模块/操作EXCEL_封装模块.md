# win32com操作excel

# 封装模块

```
# coding = utf-8
# 模块及版本要求：pywin32==227

import win32com.client

class Win32Model(object):
    def __init__(self, file_path):
        # 调用APP
        self.xlApp = win32com.client.DispatchEx("Excel.Application")
        self.xlApp.Visible = 0  # 显示EXCEL界面
        self.xlApp.DisplayAlerts = 0

        self.__open(file_path)  # 打开EXCEL文件

    # 打开文件
    def __open(self, file_path):
        try:
            self.xlBook = self.xlApp.Workbooks.Open(file_path, False)
            return "文件打开成功"
        except Exception as error:
            self.xlBook = False
            print(error)
            return error

    # 运行VBA
    def useVBA(self, VBA):
        """
        运行文件中VBA程序
        :param VBA:VBA程序名称
        :return:运行结果
        """
        try:
            self.xlBook.Application.Run(VBA)  # 宏,VBA宏名字
            return "【{}】宏运行成功".format(VBA)
        except Exception as error:
            print(error)
            return error

    # 指定单元格区域添加数据
    def add(self, sheet, rang, value):
        """
        更新excel表单元格内容
        :param sheet:sheet名字
        :param rang:单元格
        :param value:更新值
        :return:
        """
        try:
            sht = self.xlBook.Worksheets(sheet)  # 打开对应名称的sheet
            sht.Range(rang).Value = value
            return "{0}【{1}】数据添加成功".format(sheet, rang)
        except Exception as error:
            print(error)
            return error

    # 读取sheet表数据
    def selece(self, sheet, rang="UsedRange"):
        """
        读取sheet表数据
        :param sheet: sheet表名字
        :param rang: 读取单元格，默认已使用区域，可设置读取单元格区域，例：A1:D3
        :return: 单元格值
        """
        try:
            sht = self.xlBook.Worksheets(sheet)  # 打开对应名称的sheet
            if rang == "UsedRange":
                return sht.UsedRange.Value
            else:
                return sht.Range(rang).Value
        except Exception as error:
            print(error)
            return error

    # 删除指定行到最后一行数据
    def delete(self, sheet, start_row):
        """
        按照表格行删除最后几行数据
        :param sheet: sheet表名字
        :param start_row: 起始行
        :return:
        """
        try:
            sht = self.xlBook.Worksheets(sheet)  # 打开对应名称的sheet
            nrows = sht.UsedRange.Rows.Count  # 获取使用区域的行数
            if nrows >= start_row:
                sht.Rows("{}:{}".format(start_row, nrows)).ClearContents()  # 对当前使用区域清除内容
                return "{0}【{1}:{2}】行数据删除成功".format(sheet, start_row, nrows)
            else:
                return "最大行{1}小于起始行{0}，删除失败".format(start_row, nrows)
        except Exception as error:
            print(error)
            return error

    # 关闭对象Book、App
    def __del__(self):
        """
        退出程序函数
        :return:
        """
        if self.xlBook:
            self.xlBook.Save()
            self.xlBook.Close()
        self.xlApp.Quit()
        print("Excel操作完成")


    # 其它
    """
     sht.UsedRange.ClearContents()  # 对当前使用区域清除内容
     nrows = sht.UsedRange.Rows.Count  # 获取使用区域的行数
     sht.UsedRange.Copy()  # 复制
     sht.Activate()  # 激活当前工作表
    """
```


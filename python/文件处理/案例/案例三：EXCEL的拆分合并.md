EXCEL的拆分合并

```
# coding=UTF-8
# Filename: excel_split _merge.py
"""
公司python培训之代码开发考试题：EXCEL的拆分合并
工具作用：
1、根据某一分类（具体哪一列可由用户指定，填写一个数据即可），将Excel分拆成多个sheet
2、将多个sheet合并成一个sheet(前提：sheet表头相同情况下)
3、将多个Excel合并成一个Excel(前提：sheet表头相同情况下)
文件格式要求：文件格式Excel
"""
import os
import sys
import xlwings as xw
import pandas as pd


class ExcelSplitMerge:
    def __init__(self):
        self.app = xw.App(visible=False, add_book=False)
        self.app.display_alerts = False
        self.app.screen_updating = False

    def featureselection(self):
        """
        工具功能选择
        :return: 选择结果
        """
        while True:
            print(
                """
                ============================================================================
                =                                工具功能：                                  =
                =    1、根据某一分类（具体哪一列可由用户指定，填写一个数据即可）,将Excel分拆成多个sheet  =
                =    2、将多个sheet合并成一个sheet(前提:sheet表头相同情况下)                      =
                =    3、将多个Excel合并成一个Excel(前提:sheet表头相同情况下)                      =
                =    文件格式要求:文件格式Excel                                                =
                ============================================================================
                """
            )
            while True:
                id = str(input("请根据需要输入相应序号，每次只能选择其中一个功能使用,结束程序请输入0:"))
                if id in ["1", "2", "3"]:
                    while True:
                        ack = str(input("你已经选择功能{},确认使用请输入1,退出程序请输入0,重新选择功能请输入9:".format(id)))
                        if ack == "1":
                            return id
                        elif ack == "0":
                            sys.exit(0)
                        elif ack == "9":
                            break
                        else:
                            print("你已经选择功能{},确认使用输入有误请重新输入".format(id))
                            continue
                elif id == "0":
                    sys.exit(0)

                else:
                    print("功能选择有误请重新输入")
                    break

    def excelsplitmoresheet(self, file_path):
        """
        根据某一分类（具体哪一列可由用户指定，填写一个数据即可），将Excel分拆成多个sheet
        :param file_path: 需要拆分Excel路径
        :return: 处理结果
        """
        workbook = self.app.books.open(file_path)
        try:
            worksheet = workbook.sheets[0]
            value = worksheet.range('A1').options(pd.DataFrame, header=1, index=True, expand='table').value
            classification = input("请从下面列名中\n{}\n选取一列分进行类拆分,\n请输入列名：".format(value.columns.tolist()))

            processing_results = value.groupby(classification)
            for idx, group in processing_results:
                new_worksheet = workbook.sheets.add(idx, after=worksheet)
                new_worksheet['A1'].options(idex=False).value = group
            workbook.save()
            result = "运行成功"
        except BaseException as e:
            result = "运行失败,\n错误信息：{}".format(e)
        finally:
            workbook.close()
            self.app.quit()
        return result

    def moresheetmerge(self, file_path):
        """
        将多个sheet合并成一个sheet(前提:sheet表头相同情况下)
        :param file_path:需要合并sheet的Excel路径
        :return:处理结果
        """
        workbook = self.app.books.open(file_path)
        new_sheet_name = 'sheet合并'  # 指定合并后的新工作表名称

        try:
            all_data = pd.DataFrame()
            for i in workbook.sheets:
                values = i.range('A1').options(pd.DataFrame, header=1, index=True, expand='table').value
                all_data = pd.concat([all_data, values], axis=0)
            new_worksheet = workbook.sheets.add(new_sheet_name)
            new_worksheet['A1'].options(idex=False).value = all_data
            workbook.save()
            result = "运行成功"
        except BaseException as e:
            result = "运行失败,\n错误信息：{}".format(e)
        finally:
            workbook.close()
            self.app.quit()

        return result

    def moreexcelmerge(self, file_paths):
        """
        将多个Excel合并成一个Excel(前提：sheet表头相同情况下)
        :param file_paths: [需要合并Excel的路径列表,合并后文件存放路径]
        :return: 处理结果
        """
        all_data = pd.DataFrame()
        try:
            for file_path in file_paths[0]:
                workbook = self.app.books.open(file_path)
                try:
                    for i in workbook.sheets:
                        values = i.range('A1').options(pd.DataFrame, header=1, index=True, expand='table').value
                        all_data = pd.concat([all_data, values], axis=0)
                finally:
                    workbook.close()
            all_data.to_excel(r"{}\汇总文件.xlsx".format(file_paths[1]), encoding="openpyxl")
            result = "运行成功"
        except BaseException as e:
            result = "运行失败,\n错误信息：{}".format(e)
        finally:
            self.app.quit()

        return result


if __name__ == '__main__':

    excelsplitmerge = ExcelSplitMerge()
    id = excelsplitmerge.featureselection()

    if id == "1":
        file_path = input("请输入需要拆分的EXCEL文件路径：")
        result = excelsplitmerge.excelsplitmoresheet(file_path)
        print(result)
    elif id == "2":
        file_path = input("请输入需要合并sheet表的EXCEL文件路径：")
        result = excelsplitmerge.moresheetmerge(file_path)
        print(result)
    elif id == "3":
        file_paths = []
        file_path = input("请输入需要合并Excel文件的路径\n(注意将需要合并的Excel文件单独放到一个文件夹)：")
        for curDir, dirs, files in os.walk(file_path):
            for file in files:
                if file.startswith('~$'):  # 判断是否有文件名以“~$”开头的文件
                    continue  # 跳出本次循环
                file_paths.append(os.path.join(curDir, file))
        result = excelsplitmerge.moreexcelmerge([file_paths, file_path])
        print(result)
    else:
        print("程序功能选择存在BUG，请核查程序功能选择函数")
        sys.exit(0)

```


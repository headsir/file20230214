## python调用7zip命令密码解压缩

更新与20230531

```
# coding = utf-8
"""
程序功能：
    python调用7zip命令密码解压缩（电脑安装7Z程序）
"""
# 解压缩说明
"""
sysstr = "\""+zipSysDir+"\""+" x "+"\""+filepath+"\""+" -o"+"\""+outpath+"\""+" -p"+password
其实执行的系统命令是:"7z.exe" x "$filepath" -o"$outpath" -p$password
7z.exe $filepath $outpath加“引号”是解决路径带空格导致系统读取参数不正确问题；

具体参数介绍如下：
　x表示解压（a压缩）；
　$filepath是压缩文件路径；
　-o解压到的文件夹（-o后无空格）;
　-p压缩密码（-p后无空格）；　
　-y不提示按默认执行（如解压文件存在则覆盖）
"""
import os


class Call7Z:
    def __init__(self, path):
        # zipSysDir：7-zip安装的系统目录
        path = path.replace("/", "\\")
        self.zipSysDir = r"{}\7z.exe".format(path)

    # 解压缩
    def uncompress(self, inputpath, outpath, password="0001"):
        """
        调用7zip命令密码解压缩
        :param inputpath: 解压缩的文件存放目录或文件
        :param outpath: 解压后文件存放位置
        :param password: 压缩文件的解压密码
        :return:
        """

        files = []  # 存放需要解压缩的文件
        files_er = []  # 存放非zip、7z文件（不能解压缩）
        # 判断【inputpath】是目录还是文件 os.path.isfile(inputpath) 是文件，返回True
        if os.path.isfile(inputpath):
            # 如果是文件，添加进列表
            files.append(inputpath)
        else:
            # 如果是目录，遍历目录中的文件后添加进列表
            for i in os.listdir(inputpath):
                files.append(i)
        # 遍历【files】列表
        for file in files:
            # 如果文件后缀是zip、7z
            if os.path.splitext(file)[1] in [".zip", ".7z"]:
                filepath = os.path.join(inputpath, file)
                # 拼接解压缩命令
                sysstr = "\"" + self.zipSysDir + "\"" + " x " + "\"" + filepath + "\"" + " -o" + "\"" + outpath + "\"" + " -p" + password
                # 执行解压缩命令
                os.popen(sysstr)
            else:
                files_er.append(file)
                # print(f"{str(files_er)}不能解压缩")
                # continue
        # 抛出错误
        raise Exception(f"{str(files_er)}不能解压缩")

    # 压缩
    def compress(self, outfile, *inputpath):
        """
        调用7zip压缩
        :param inputpath:需要压缩的文件或目录,如果参数是列表在前面加 * 号
        :param outfile:压缩后文件
        :return:
        """
        # 压缩命令拼接
        sysstr = "\"" + self.zipSysDir + "\"" + " a " + "\"" + outfile + "\"" + ' "' + "\" \"".join(inputpath) + '"'
        # 执行压缩命令
        os.popen(sysstr)


if __name__ == '__main__':
    # 使用方法
    Call7Z_path = r"D:\Program Files\7-Zip"
    z = Call7Z(Call7Z_path)
    try:
        z.compress("D:/桌面/零流量.zip", *[r"D:\桌面\工作附件", r"D:\桌面\共享"])
        # z.uncompress(r"D:\数据库\河南电信省公司项目\日报\1", r"D:\数据库\河南电信省公司项目\日报\2")
    except Exception as e:
        print(e)

```
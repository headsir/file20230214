## python调用7zip命令密码解压缩

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

    def uncompress(self, outpath, filepath, password="0001"):
        """
        调用7zip命令密码解压缩
        :param outpath: 解压缩的文件存放目录
        :param filepath: 解压后文件存放位置
        :param password: 压缩文件的解压密码
        :return:
        """

        # 解压缩命令
        sysstr = "\"" + self.zipSysDir + "\"" + " x " + "\"" + filepath + "\"" + " -o" + "\"" + outpath + "\"" + " -p" + password
        os.popen(sysstr)

    def compress(self, outpath, filepath):
        """
        调用7zip压缩
        :param outpath:需要压缩的文件存放目录
        :param filepath:压缩后文件存放位置
        :return:
        """
        # 压缩命令
        sysstr = "\"" + self.zipSysDir + "\"" + " a " + "\"" + filepath + "\"" + " \"" + outpath + "\""
        os.popen(sysstr)

# if __name__ == '__main__':
#     Call7Z_path = r"D:\Program Files\7-Zip"
#     z = Call7Z(Call7Z_path)
#     z.compress("D:/桌面/20220722", "D:/桌面/999")
#     # z.uncompress("D:/桌面/1", "D:/桌面/新建文件夹.zip")

```
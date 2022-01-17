## MSVCR100.dll缺失

### 官方方案

你可以尝试以下方案，看看是否有效：

在管理员命令提示符下键入以下命令：

 Dism /Online /Cleanup-Image /ScanHealth

 这条命令将扫描全部系统文件并和官方系统文件对比，扫描计算机中的不一致情况。

 Dism /Online /Cleanup-Image /CheckHealth

 这条命令必须在前一条命令执行完以后，发现系统文件有损坏时使用。

 DISM /Online /Cleanup-image /RestoreHealth

 这条命令是把那些不同的系统文件还原成官方系统源文件。

 完成后重启，再键入以下命令：sfc /SCANNOW

# win10的directX安装

建议您可以尝试按住Win+R,输入services.msc，回车，查看Windows Installer服务是否有开启，将其启动类型改为启动，点击依存关系选项卡，查看相关依存关系服务将启动类型改为启动。

按住Windows徽标+R，输入 dxdiag，DirectX 诊断工具诊断一下
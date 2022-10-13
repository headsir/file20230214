CRT一直持续收费，偶尔一些需要用到免费ssh工具的时候，会首先考虑使用Xshell，最近在网上看到了一款免费的ssh工具，FinalShell，初步测试了下，用起来是真的很舒服。

## 一.FinalShell介绍

**官网:**

**主要特性:**

```
1.多平台支持Windows,macOS,Linux
2.多标签,批量服务器管理.
3.支持登录ssh和Windows远程桌面.
4.漂亮的平滑字体显示,内置100多个配色方案.
5.终端,sftp同屏显示,同步切换目录.
6.命令自动提示,智能匹配,输入更快捷,方便.
7.sftp支持,通过各种优化技术,加载更快,切换,打开目录无需等待.
8.服务器网络,性能实时监控,无需安装服务器插件.
9.内置海外服务器加速,加速远程桌面和ssh连接,操作流畅无卡顿.
10.内存,Cpu性能监控,Ping延迟丢包,Trace路由监控.
11.实时硬盘监控.
12.进程管理器.
13.快捷命令面板,可同时显示数十个命令.
14.内置文本编辑器,支持语法高亮,代码折叠,搜索,替换.
15.ssh和远程桌面均支持代理服务器.
16.打包传输,自动压缩解压.
17.支持rz,sz (zmodem)
18.多地点ping监控
19.命令输入框,支持自动补全,命令历史
20.自定义命令参数功能,可动态根据输入参数生成命令
21.可设置终端背景图片,并拥有动态背景模糊,文字阴影效果.
22.一键查看各种系统信息
```

**下载地址:**

```
Windows版下载地址:
http://www.hostbuf.com/downloads/finalshell_install.exe
Mac版,Linux版安装及教程:
http://www.hostbuf.com/t/1059.html

更新日志:
http://www.hostbuf.com/t/989.html
```

## 二.初步使用

1.新建一个连接

![image-20221012142254161](imge/FinalShell.assets/image-20221012142254161.png)



![img](https://upload-images.jianshu.io/upload_images/2638478-468c88634503b680.png?imageMogr2/auto-orient/strip|imageView2/2/w/609/format/webp)

![img](https://upload-images.jianshu.io/upload_images/2638478-44a1b69b0b95f273.png?imageMogr2/auto-orient/strip|imageView2/2/w/680/format/webp)



1. 这个UI界面也真的很棒

   ![img](https://upload-images.jianshu.io/upload_images/2638478-61a3e16f16dfea0e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1180/format/webp)

   

3.直接查看资源使用率
 不用敲命令，直接可以看到资源使用率和网络相关的，确实比较方便

![img](https://upload-images.jianshu.io/upload_images/2638478-b630e5089598fb40.png?imageMogr2/auto-orient/strip|imageView2/2/w/1133/format/webp)

4.高级功能需要会员收费
 高级功能例如系统信息，进程管理，需要会员登录



![img](https://upload-images.jianshu.io/upload_images/2638478-029a743ebbf177ac.png?imageMogr2/auto-orient/strip|imageView2/2/w/978/format/webp)

## 三、Linux学习

### 常用命令

ls 显示指定目录下内容

-  -a 显示所有文件及目录
- -l 列出文件详细信息，ls -l 简写 ll
- -la

cd 切换目录

pwd 查看所处目录

. 表示当前路径（隐藏文件）

.. 当前目录的上一级

~ 

/ 根目录

history 显示历史执行命令

Tab键 补全命令

shutdown -r 重启电脑

### 创建、删除

mkdir 创建目录

- -p 父目录不存在，自动创建

touch 创建空文件

rm 删除文件或目录（空目录）

- -f 强制直接删除，无需确认
- -r 将目录及以下所有递归逐一删除

### 复制、移动

cp 复制文件或目录

- -r 复制目录下的所有文件

mv 文件或目录改名或移入其它位置

### 文件内容查看

cat 文件输出到控制台，适合小文件

more 文件逐页显示，适合大文件，空格键向下翻页，回退向上翻

tail 查看文件结尾部分

- n 用于显示行数，默认10
- -f 实时显示文件动态追加的内容

### 其它

echo 用于内容输出

| 管道符号，先执行前面内容，把结果给后面命令

‘> 输出重定向（覆盖）

‘>> 输出重定向（追加）

### 解压缩命令

打包、解包

tar 

- -c  打包，创建文件
- -x 解包
- -v 显示执行过程
- -f 指定文件
- -C 指定目录(默认当前路径)
- -z 压缩算法，.tar.gz(tgz)

### 时间、日期查看

data 显示 或设定系统日期与时间，用+设定显示格式

cal 显示当前或指定日期的公历

### 内存、磁盘使用率

free 显示内存使用率

df 显示磁盘使用率

参数：-h 

### 进程查看

ps 显示当前进程状态，参数 -ef

kill -9 进程号 杀死进程

jps 专门查看java进程

### 服务器基础环境准备

#### 主机名

vim /etc/hostname 修改主机名

cat /etc/hostname 查看主机名

#### Hosts 映射

cat /etc/hosts 查看主机映射

### 防火墙关闭

systemctl stop firewalld.service  关闭防火墙

systemctl disable firewalld.service  禁止防火墙开启自启

systemctl status firewalld.service  防火墙状态

## ssh免密登录（node1执行- node1|node2|node3)

ssh-keygen #4个回车 生成公钥、私钥

ssh-copy-id nodel、ssh-copy-id node2、ssh-copy-id node3、







## 四、VIM编辑器

i 输入模式，定位当前光标前面

o 输入模式，定位到当前行的下一行（添加一行空行）

:q 退出

:w 保存

:wq 保存退出

:wq! 强制保存退出

:q! 强制退出

shift + zz 命令模式下快速保存退出

### 光标移动操作

home 行首 end 行尾

G 跳到文件的最后一行

gg 跳到文件第一行

### 复制粘贴

yy 复制光标当前所在行

nyy 复制当前行往下n行

p 粘贴到当前行的下一行

P 粘贴到当前行的上一行

### 删除、撤销操作

dd 删除光标所在当前行

ndd 删除当前行往下n行

u 撤销上一步操作

Ctrl + r 反撤销

### :one:下载：

> https://cn.ubuntu.com/download/desktop

![image-20240729184106309](imge/Ubuntn安装.assets/image-20240729184106309.png)

### :two:安装

个别步骤注意，其他默认

![image-20240729184010306](imge/Ubuntn安装.assets/image-20240729184010306.png)

![image-20240729184213071](imge/Ubuntn安装.assets/image-20240729184213071.png)

![image-20240729184333167](imge/Ubuntn安装.assets/image-20240729184333167.png)

![image-20240729184606458](imge/Ubuntn安装.assets/image-20240729184606458.png)

### :three:安装tool

`sudo  ./vmware-install.pl`

### :four:解决不能拖拽复制及共享问题

```
sudo vi /etc/gdm3/custom.conf
sudo apt install open-vm-tools-desktop
sudo /usr/bin/vmhgfs-fuse .host:/ /mnt/hgfs -o subtype=vmhgfs-fuse,allow_other
```

# 使用笔记

## 软件相关

### 卸载软件

```
sudo apt-get autoremove --purge python3
autoremove:删除所有自动安装且其不再需要的依赖包
--purge：删除配置文件
```

### 查看已安装的包

```
apt list --installed
```

## git 相关

安装git : `sudo apt install git`

配置git:

```
git config --global user.name "disguiser" 
git config --global user.email "978345836@qq.com"
```

创建文件夹并初始化 Git 存储库

现在，让我们为项目创建一个新文件夹并初始化一个 Git存储库。

```text
$ mkdir project_code
$ cd project_code
$ git init
```

进行更改后，即可将其提交到 Git 存储库：

```text
$ git add .
$ git commit -m "First Commit"
```

此命令暂存所有更改，并提交描述更改的消息。

生成SSH公钥：

公钥文件位置 `cat ~/.ssh/id_rsa.pub`

```
ssh-keygen -t rsa -C 978345836@qq.com
```

具体参考：[](D:\数据库\记事本\study-notes\github\github 公钥配置.md)

关联远程仓库

```
$ git remote add origin git@github.com:pradeepantil/project_code.git
$ git remote -v
```

最后，将本地更改推送到 存储库，运行“git push”命令。

```text
$ git push -u origin master
```

远程仓库拉取到本地

```
$ git fetch origin
$ git merge origin/master
```

## python相关

安装pip 

```
sudo apt install python3-pip
```

安装 `Uwsgi`

```
sudo apt install python3 uwsgi
```






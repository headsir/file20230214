# 一、系统配置：

## 1.1 设置计算机名称

- RedHat的hostname修改

​			vi   /etc/sysconfig/network 

​			HOSTNAME=你要设置的hostname

- Debian发行版hostname修改

​			vi  /etc/hostname 修改主机名

​			cat /etc/hostname 查看主机名

重启系统读取配置文件设置新的hostname

## 1.2 编辑网络配置文件

- 修改配置文件 ifcfg-ens33 命令 vi /etc/sysconfig/network-scripts/ifcfg-ens33

  TYPE="Ethernet" #网卡类型为以太网卡 ==需要==
  PROXY_METHOD="none"
  BROWSER_ONLY="no"
  BOOTPROTO="staitc"    # dhcp 自动分配ip ，staitc 静态ip。==必须修改==
  DEFROUTE="yes"
  IPV4_FAILURE_FATAL="yes"
  IPV6INIT="yes"
  IPV6_AUTOCONF="yes"
  IPV6_DEFROUTE="yes"
  IPV6_FAILURE_FATAL="no"
  IPV6_ADDR_GEN_MODE="stable-privacy"
  NAME="ens33" # 网卡名称 ==需要==
  UUID="e8a9dda6-1f3d-416b-802f-16fcc4b4761c" # 网卡设备编号，可以删除
  DEVICE="ens33" # 网卡设置 ==需要==
  ONBOOT="yes"  激活网卡 ==必须修改==
  IPADDR="192.168.88.151"  # ip地址  ==必须添加==
  PREFIX="24" # 掩码 ==必须添加==
  GATEWAY="192.168.88.2" #网管 ==必须添加==
  DNS1="192.168.88.2" # 域名服务器1 ==必须添加==
  DNS2="114.114.114.114" # 域名服务器2 ==必须添加==
  IPV6_PRIVACY="no"

- 重启服务 sudo systemctl restart network / service network restart

## 1.3 Hosts 映射

- 修改配置文件hosts 命令 vi /etc/hosts

  添加内容

  ```
  # IP地址			别名	 网址
  192.168.88.151 centos1 centos.centos1.cn
  192.168.88.152 centos2 centos.centos2.cn
  192.168.88.153 centos3 centos.centos3.cn
  ```

- 查看主机映射 cat /etc/hosts 

## 1.4 安装 vim 和 ntp（集群时间同步）

- ntp :时间同步 yum install ntp -y  ‘-y’ 可以省略确认提示

  依赖 ntpdate 会同步安装

  date 查看系统时间

  chkconfig 查看服务状态  举例：chkconfig ntp on 服务状态打开

  - 3 命令行启动 
  - 5 图形界面启动
  - 6 重启
  - 0 关机
  - 2 无网络用户
  - 1 单用户模式
  - 4 未用

  >  集群时间同步 ==推荐==
  >
  > yum -y install ntpdate  安装插件
  >
  > ntpdate ntp5.aliyun.com  阿里云时间授权

- vim :编辑器

## 1.5 防火墙关闭

systemctl stop firewalld.service  关闭防火墙

systemctl disable firewalld.service  禁止防火墙开启自启

systemctl status firewalld.service  防火墙状态

## 1.6 禁用 selinux

增强安全子系统，不需要，关掉

getenforce 查看状态

编辑配置文件 vim /etc/selinux/config 

SELINUX=enforcing  修改为 disabled

重启生效

## 1.7 ssh免密登录

（node1执行- node1|node2|node3)

ssh-keygen #4个回车 生成公钥、私钥

ssh-copy-id nodel、ssh-copy-id node2、ssh-copy-id node3 可以把本地主机的公钥复制到远程主机的authorized_keys文件上（推荐）

 scp -r authorized_keys root@hadoop02:/root/.ssh/ 远程传输文件

## 1.8 关闭 sshd 服务的DNS 【非必须】

加快 SSH登录远程的速度 

![image-20221022024317022](imge/2Linux学习.assets/image-20221022024317022.png)

![image-20221022024331667](imge/2Linux学习.assets/image-20221022024331667.png)

## 1.9 删除 70-persistent-net.rules文件

文件路径 /etc/udev/rules.d

![image-20221022025209539](imge/2Linux学习.assets/image-20221022025209539.png)

![image-20221022025230435](imge/2Linux学习.assets/image-20221022025230435.png)

## 1.10 创建统一工作目录

mkdir -p /export/server/  软件安装路径

mkdir -p /export/data/  数据储存路径

mkdir -p /export/software/  安装包存放路径

# 简介

- paramiko是一个基于SSHv2协议的纯Python（2.7，3.4+）库；
- 提供了客户端和服务器的功能；
- 可以实现SSH2远程安全连接，支持认证和密钥方式；
- 一般用于执行远程命令、传输文件、中间SSH代理等。

paramiko可以在Python代码中直接使用SSH协议对远程服务器进行操作，而不是调用ssh命令对远程服务器进行操作。

# 安装

```
pip install paramiko
```

# 说明

paramiko包含两个核心组件，分别是SSHClient和SFTPClient。

## SSHClient类

SSHClient类是对SSH会话的封装，该类封装了传输（transport）、通道（channel）及SFTPClient建立的方法（open_sftp），通常用于执行远程命令。

### 方法

- **connect()** - 实现远程服务器的连接与认证，hostname是必传的参数

  ```python
  connect(self, hostname, port=22, username=None, password=None, pkey=None, key_filename=None, timeout=None, allow_agent=True, look_for_keys=True, compress=False)
  ```

  参数说明：

  - hostname(str类型)：连接目标主机IP地址或主机名
  - port(int类型)：连接目标主机的端口，默认为22
  - username(str类型)：校验的用户名(默认为当前的本地用户名)
  - password(str类型)：密码用于身份校验或解锁私钥
  - pkey：私钥方式用于身份验证
  - key_filename(str or list(str)类型)：一个文件名或文件名的列表，用于私钥的身份验证
  - timeout(float类型)：一个可选的超时时间(以秒为单位)的TCP连接
  - allow_agent(bool类型)：设置为False时用于禁用连接到SSH代理
  - look_for_keys(bool类型)：设置为False时用来禁用在~./ssh中搜索秘钥文件
  - compress(bool类型)：设置为True时打开压缩

- **set_missing_host_key_policy(policy)** - 设置连接远程主机没有本地主机秘钥或HostKeys对象时的策略，目前支持如下三种方式

  - AutoAddPolicy：自动添加主机名及主机秘钥到本地HostKeys对象，并将其保存，不依赖load_system_host_keys，即使~/.ssh/known_hosts不存在也不影响
  - RejectPolicy(默认)：自动拒绝未知的主机名或秘钥，依赖load_system_host_keys()配置
  - WarningPolicy：用于记录一个未知的主机秘钥的Python警告，并接受它，功能上与AutoAddPolicy相似，但未知主机会有告警

- **exec_command()** - 在远程服务器上执行Linux命令的方法

- **open_sftp()** - 在当前ssh会话的基础上创建一个sftp会话。该方法会返回一个SFTPClient对象

- **load_system_host_keys()** - 加载本地公钥校验文件，默认为~/.ssh/known_hosts,非默认路径需要收工指定

### 例子

```
import paramiko

SSH_CONFIG = {
    'hostname': '10.10.31.12',  # ip
    'port': 22,  # 端口
    'username': 'root',  # 用户名
    'password': '123456',  # 密码
}


def main():
    # 建立ssh连接
    ssh_client = paramiko.SSHClient()  # 实例化SSHClient对象
    ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy())  # 设置自动添加策略
    ssh_client.connect(**SSH_CONFIG)  # 连接SSH服务端，以用户名和密码进行认证

    # 开启一个Channel并执行命令
    stdin, stdout, stderr = ssh_client.exec_command('uname -a')  # stdout为正确输出，stderr为错误输出，同时只有1个变量有值
    print(stdout.read().decode('utf-8'))  # 打印返回的stdout
    ssh_client.close()  # 关闭SSHClient


if __name__ == '__main__':
    main()
```



## SFTPClient类

SFTPClient作为一个SFTP客户端对象，根据SSH传输协议的sftp会话，实现远程操作，比如文件上传，下载，权限，状态等，端口就是SSH端口

### 方法

- **from_transport()**：创建一个已连通的SFTP客户端通道
- **put()**：上传本地文件到远程服务器
- **get()**：从远程服务器下载文件到本地
- **mkdir()**：在远程服务器上创建目录
- **remove()**：删除远程服务器中的文件
- **rmdir()**：删除远程服务器中的目录
- **rename()**：重命名远程服务器中的文件或目录
- **stat()**：获取远程服务器中文件的详细信息
- **listdir()**：列出远程服务器中指定目录下的内容

### 例子

```
import paramiko


def main():
    tran = paramiko.Transport(('10.10.31.12', 22))  # 获取Transport实例
    tran.connect(username="root", password='123456')  # 连接SSH服务端
    sftp_client = paramiko.SFTPClient.from_transport(tran)  # 实例化SFTPClient对象

    # 设置上传的本地/远程文件路径
    local_path = "D:/doc/a.txt"
    remote_path = "/tmp/a.txt"

    sftp_client.put(local_path, remote_path)  # 上传
    print("上传成功")
    sftp_client.get(remote_path, local_path)  # 下载
    print("下载成功")
    tran.close()  # 关闭


if __name__ == '__main__':
    main()
```
# Hadoop 源码编译

## 一、java安装

下载：https://www.oracle.com/java/technologies/downloads/#java8  【jdk-8u351-linux-x64.tar.gz】

## 上传、解压 JDK 1.8安装包

tar -zxvf jdk-8u351-linux-x64.tar.gz 解压文件安装

​	配置环境变量

​	vi /etc/profile 编辑环境变量文件

```
export JAVA_HOME=/export/server/jdk1.8.0_351
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

重新加载环境变量文件

 source /etc/profile

检查是否安装成功

java -version

## 二、安装maven

进入到maven的conf目录：cd /root/apps/apache-maven-3.5.3/conf

修改配置文件settings.xml：

在配置文件的中部找到localRepository这个标签，是被注释掉的，将其放出      来自己添加路径:

创建仓库并配置

```powershell
# mkdir repo 创建仓库
<localRepository>/export/server/apache-maven-3.8.6/repo</localRepository>  settings.xml配置仓库
```

在 mirrors节点中添加阿里云镜像

```
  <mirrors>

   <mirror>

       <id>nexus-aliyun</id>

        <mirrorOf>central</mirrorOf>

        <name>Nexus aliyun</name>

       <url>http://maven.aliyun.com/nexus/content/groups/public</url>

   </mirror>

  </mirrors>
```

配置环境变量

​	vi /etc/profile 编辑环境变量文件

```
export MAVEN_HOME=/export/server/apache-maven-3.8.6
export PATH=$PATH:$MAVEN_HOME/bin
```

重新加载环境变量文件

 source /etc/profile

检查是否安装成功

mvn -version

## 三、安装相关的依赖

注意安装依赖顺序不可乱,可能会出现依赖找不到问题

\# 安装gcc make 

yum install -y gcc* make

 

\#安装压缩工具 		

yum -y install snappy*  bzip2* lzo* zlib*  lz4* gzip*

 

\#安装一些基本工具 		

yum -y install openssl* svn ncurses* autoconf automake libtool

 

\#安装扩展源,才可安装zstd

yum -y install epel-release

\#安装zstd

yum -y install *zstd*

## 四、安装cmake

解压安装：tar -zxvf /root/cmake-2.8.12.2.tar.gz

编译安装：

进入根目录：cd /root/apps/cmake-2.8.12.2/

依次执行一下命令（比较耗时）：

./bootstrap 

gmake 

 gmake install

检查安装是否成功：cmake -version

## 五、安装protobuf

解压安装：tar -zxvf /export/server/protobuf-2.6.0.tar.gz

编译安装：

进入根目录：cd /root/apps/protobuf-2.6.0

依次执行下列命令 --prefix 指定安装到当前目录 		

./configure --prefix=/export/server/protobuf-2.6.0

 make && make install

 

\#配置环境变量 		

 vim /etc/profile

 ```
 export PROTOC_HOME=/export/server/protobuf-2.6.0
 export PATH=$PATH:$JAVA_HOME/bin:$MAVEN_HOME/bin:$PROTOC_HOME/bin
 ```

重新加载环境变量文件

source /etc/profile

检查是否安装成功

protoc --version

## 六、编译源码

解压安装：tar -zxvf /export/server/hadoop-3.3.4-src.tar.gz

编译安装：

进入根目录：cd /export/server/hadoop-3.3.4-src

执行下列命令

mvn clean package -DskipTests -Pdist,native -Dtar

编译过程会非常漫长，第一次编译需要下载很多依赖jar包,最终所有的模块全部SUCCESS

成功的64位hadoop包位/hadoop-3.3.4-src/hadoop-dist/target下的hadoop-3.3.4.tar.gz压缩包

# Hadoop 安装

解压安装：tar -zxvf hadoop-3.3.4.tar.gz

![image-20221022235925461](imge/3Hadoop学习.assets/image-20221022235925461.png)
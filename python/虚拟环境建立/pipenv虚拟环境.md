# pipenv虚拟环境

## 一、pipenv虚拟环境目录设置

新建一个名为“ WORKON_HOME ”的环境变量，然后将环境变量的值设置为指定路径

![image-20220117164015304](imge\pipenv虚拟环境.assets\image-20220117164015304.png)

## 二、使用方法

### 1、安装 pipenv

`pip install pipenv`

### 2、创建虚拟环境

```
# 创建虚拟环境
pipenv install 

pipenv --python 3.7  # 也可指定具体版本
```

说明：会自动更新配置文件(安装pipfile里面的模块)

### 3、虚拟环境包管理

#### 3.1 安装包

```
pipenv install -e . "" # 安装本地包到虚拟环境
pipenv install 模块  # 模块安装
pipenv install --dev  # 安装所有依赖
pipenv install -r requirements.txt  # 导入requirements.txt 需要转码为UTF-8

官方		https://pypi.org/simple
华为镜像源  https://mirrors.huaweicloud.com/
阿里云 https://mirrors.aliyun.com/pypi/simple/
中国科学技术大学 https://pypi.mirrors.ustc.edu.cn/simple/
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
浙江大学开源镜像站    https://mirrors.zju.edu.cn/
腾讯开源镜像站    https://mirrors.cloud.tencent.com/pypi/simple
豆瓣 https://pypi.douban.com/simple/
网易开源镜像站    https://mirrors.163.com/
搜狐开源镜像    https://mirrors.sohu.com/
大连东软镜像				http://mirrors.neusoft.edu.cn/pypi/web/simple/
```

#### 3.2 更新包

```
pipenv update 模块名  # 更新指定包
pipenv update  # 更新所有包
pipenv update --outdated  # 打印所有要更新的包
```

#### 3.3 卸载包

```
pipenv  uninstall  # 模块卸载
pipenv  uninstall --all  # 卸载全部包
```

#### 3.4 导出包

导出requirements.txt

```
导出虚拟环境安装的包
方法1：pipenv run pip freeze > requirements.txt # 相当于【pipenv run pip freeze】
方法2：pipenv lock -r > requirements.txt

导出开发用的包：pipenv lock -r --dev > requirements.txt
```

#### 3.5 其它

```
pipenv graph  # 显示包依赖关系
pipenv check  # 检查安全漏洞
```

如果通过 pipenv 命令安装和卸载 package，安装或卸载完成后还会更新 `Pipfile.lock` 文件，有时候会卡在这个步骤。通常可以 `ctrl+c` 强制推出，删除 `Pipfile.lock`, 然后

```bash
pipenv lock
```

重新生成该文件

### 4、虚拟环境

```text
# 进入虚拟环境
pipenv shell
# 退出虚拟环境
exit
```

### 5、使用方法

​	    `pipenv --venv`显示虚拟环境安装路径

​		`pipenv --where`返回项目路径

​		`pipenv --py`返回虚拟环境解释器

​		`pipenv --rm`移除虚拟环境目录






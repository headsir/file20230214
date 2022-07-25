# pipenv虚拟环境

## 一、pipenv虚拟环境目录设置

新建一个名为“ WORKON_HOME ”的环境变量，然后将环境变量的值设置为指定路径

![image-20220117164015304](imge\pipenv虚拟环境.assets\image-20220117164015304.png)

## 二、使用方法

### 1、安装 pipenv

`pip3 install pipenv`

### 2、模块安装

```
pipenv install
```

说明：会自动更新配置文件

### 3、模块卸载

```bash
pipenv  uninstall
```

如果通过 pipenv 命令安装和卸载 package，安装或卸载完成后还会更新 `Pipfile.lock` 文件，有时候会卡在这个步骤。通常可以 `ctrl+c` 强制推出，删除 `Pipfile.lock`, 然后

```bash
pipenv lock
```

重新生成该文件

### 4、进入虚拟环境

```text
pipenv shell
```

### 5、退出虚拟环境

```text
pipenv exit
```

### 6、 `requirements.txt` 文件

```bash
导出requirements.txt
方法1：pipenv run pip freeze > requirements.txt
方法2：pipenv lock -r --dev > requirements.txt

导入requirements.txt
pipenv install -r requirements.txt
```

### 7、使用方法

​		`pipenv graph` 模块查询

​		`pipenv update` 模块更新

​		`exit`退出虚拟环境

​		`pipenv graph`显示包依赖关系

​	    `pipenv --venv`显示虚拟环境安装路径


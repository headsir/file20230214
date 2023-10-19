# github 公钥配置

### **1、使用ssh-keygen命令生成ssh 密钥**

```
ssh-keygen -t rsa -C "你的邮箱"
```

说明：ssh-keygen命令可以生成rsa或dsa两种格式的密钥。在上面的示例中，使用-t rsa参数生成了id_rsa和id_rsa.pub两个文件，分别表示rsa私钥和rsa公钥。同理，可以使用-t  dsa参数生成dsa私钥和dsa公钥，生成的文件名分别是：id_dsa、id_dsa.pub。

![image-20220117132107504](imge\github 公钥配置.assets\image-20220117132107504.png)

![image-20231019153912186](imge/github 公钥配置.assets/image-20231019153912186.png)

### **2、配置SSH**

将.pub里面的全部代码复制到github的SSH中

![image-20220207144601714](imge\github 公钥配置.assets\image-20220207144601714.png)

![image-20220117132611177](imge\github 公钥配置.assets\image-20220117132611177.png)

GITEE设置

![image-20231019154255007](imge/github 公钥配置.assets/image-20231019154255007.png)

### **3、测试ssh keys是否设置成功**

```
 ssh -T git@gitee.com
```

### 4、.git/config 文件配置

![image-20220207150928858](imge\github 公钥配置.assets\image-20220207150928858.png)

```
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[submodule]
	active = .
[remote "origin"]
	url = git@gitee.com:disguisor/study-notes.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[lfs]
	repositoryformatversion = 0

```



### 5、.gitconfig 文件配置

![image-20220207151519509](imge\github 公钥配置.assets\image-20220207151519509.png)

![image-20231019153712932](imge/github 公钥配置.assets/image-20231019153712932.png)



# GitHub Desktop配置

![image-20221228103254726](imge/github 公钥配置.assets/image-20221228103254726.png)

![image-20221228103131144](imge/github 公钥配置.assets/image-20221228103131144.png)

![image-20221228103002724](imge/github 公钥配置.assets/image-20221228103002724.png)

![image-20221228103029458](imge/github 公钥配置.assets/image-20221228103029458.png)
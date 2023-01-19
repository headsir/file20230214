# VSCode之创建Python虚拟环境             

# 一、创建虚拟环境

```
#    work(文件名)
python -m venv work
```

# 二、进入虚拟环境

```
work\Scripts\activate
```

# 三、退出虚拟环境

```
deactivate
```

 

注意：

1.此系统无法运行此脚本（[PowerShell ](https://docs.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2))，更改执行策略(Set-ExecutionPolicy -ExecutionPolicy RemoteSigned)：是(Y)
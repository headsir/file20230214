## 创建虚拟环境

D:\ProgramData\python\3.12.1\python.exe -m venv .venv(必须使用.venv)

## 激活虚拟环境

./scripts/activate

## 安装uv

pip intsall uv

## 初始化项目

uv init 初始化项目

uv add 模块名  安装模块

uv pip list 查询模块

```
# 安装特定版本
uv add requests==2.31.0

# 从 requirements.txt 安装
uv add -r requirements.txt

# 安装包到开发环境：
uv add --dev pytest

# 导出当前环境的依赖
uv pip freeze > requirements.txt

# 导出生产环境依赖（排除开发依赖）
uv pip freeze --production > requirements.txt

# 安装项目的依赖：
# 直接使用现有的 pyproject.toml
uv sync
```


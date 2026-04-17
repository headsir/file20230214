# 构建Web服务

## 三层架构

![image-20260417192934833](imge/FastAPI学习手册笔记.assets/image-20260417192934833.png)

- Web层（网络层）：用来提供Web界面
- Service层（服务层）：用来实现业务逻辑
- Data层（数据层）：整个Web服务的核心组件

## 目录结构

-- src 网站所有代码都放在 src

--- web 程序的Web层

--- service 业务逻辑层

--- data 储存接口层

--- model 用来存放数据模型


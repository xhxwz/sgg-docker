docker-compose 是 Docker 官方的开源项目，负责实现对 Docker 容器集群的快速编排。

[官方文档](https://docs.docker.com/compose/compose-file/compose-file-v3/)

## 核心概念

- 一个文件 `docker-compose.yml`
- 两个要素
  - 服务 (service)：容器实例
  - 工程 (project)：由一组关联的容器组成的一个完整业务单元

## 使用步骤

- 编写 `Dockerfile` 定义各个微服务应用，并构建出对应的镜像文件
- 使用 `docker-compose.yml` 定义一个完整业务单元，安排好整体应用中各个容器服务
- 最后，执行 `docker compose up` 命令，启动并运行整个应用程序，完成一键部署上线

## 常用命令

- `up [-d]`：启动所有 docker compose 服务，`-d`表示在后台运行这些服务
- `down`：停止并删除容器、网络、卷、镜像
- `exec 服务id`：进行实例内部，比如 `docker compose exec service1 /bin/bash`
- `ps`：当前docker compose 编排运行的所有容器
- `top`：当前 docker compose 编排的容器进程
- `logs 服务id`：查看容器输出日志
- `config [-q]`：检查配置。`-q`表示有问题才输出
- `restart`：重启
- `start`：启动
- `stop`：停止


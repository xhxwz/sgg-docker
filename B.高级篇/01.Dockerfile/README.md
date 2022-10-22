[官方文档](https://docs.docker.com/engine/reference/builder/)

## 基础知识

- 每条保留字指令都必须为大写字母，且后面要跟随至少一个参数
- 指令从上到下顺序执行
- `#` 为注释
- 每条指令都会创建一个新的镜像层并对镜像进行提交

## 常用保留字

- `FROM 镜像 [AS 别名]`
- `MAINTAINER 作者 <邮箱>`
- `RUN`：容器构建(docker build)时需要运行的命令
  - shell格式：`RUN SHELL命令`，比如 `RUN apt install vim-nox -y`
  - exec格式：`RUN ["可执行文件", "参数1", "参数2"]`，比如 `RUN ["apt", "install", "vim-nox", "-y"]`
- `EXPOSE`：对外暴露的端口
- `WORKDIR`：指定在创建容器之后，终端默认登录进来的工作目录
- `USER`：指定该镜像以会用用户去执行，默认是`root`用户
- `ENV`：环境变量，可以在后续的 `RUN` 指令中使用。
- `ADD`：将宿主机目录下的文件拷贝到镜像，且会自动处理URL和解压 tar 压缩包。
- `COPY`：将宿主机目录下的文件拷贝到镜像。
- `VOLUME` ：容器数据卷
- `CMD`：指定**容器启动后要做的事**。docker run 时运行。
  - 可以有多个 `CMD`，**但只有最后一个生效**
  - **`CMD` 会被 `docker run` 指定的参数替换**
- `ENTRYPOINT`：指定容器启动后要运行的命令
  - **`ENTRYPOINT` 不会被 `docker run` 指定的参数替换**。这些命令行参数**会被当作参数送给 `ENTRYPOINT`指令指定的程序**
  - 如果有`CMD`，那么`CMD`将作为`ENTRYPOINT` 的参数

## 案例

要求：构建具备 VIM、IFCONFIG、JDK8 的 debian

> wget https://mirrors.huaweicloud.com/java/jdk/8u171-b11/jdk-8u171-linux-x64.tar.gz

```dockerfile
FROM debian:11

ENV WORKSPACE /root
WORKDIR $WORKSPACE

RUN apt-get update -y && apt-get install vim-nox net-tools -y && \
    mkdir /usr/local/java
ADD jdk-8u171-linux-x64.tar.gz /usr/local/java
ENV JAVA_HOME /usr/local/java/jdk1.8.0_171
ENV JRE_HOME $JAVA_HOME/jre
ENV PATH $PATH:$JAVA_HOME/bin
CMD /bin/bash
```

```bash
docker build . -t foo/debian-vim-jdk8
docker run -it --rm foo/debian-vim-jdk8 bash
```

## 虚悬镜像

```bash
docker images -a
# 或
docker image ls -f dangling=true
```

仓库名、标签都是 `<none>` 的镜像

### 清理

```bash
docker image prune
```


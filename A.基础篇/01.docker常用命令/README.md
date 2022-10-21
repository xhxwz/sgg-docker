## 帮助启动类命令

```bash
systemctl [start|stop|restart|status|enable] docker

# docker 概要信息
docker info

# docker 总体帮助文件
docker --help

# docker 命令帮助文件
docker 命令 --help
```

## 镜像命令

```bash
# 列出本地镜像
docker images [-a] [-q]
```

- `-a`：所有镜像
- `-q`：只显示镜像ID

```bash
# 搜索
docker search [--limit N] 镜像名称

# 拉取
docker pull 镜像名称[:tag]

# 查看镜像/容器/数据卷占用的空间
docker system df
# TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
# Images          3         2         975.5MB   164.6MB (16%)
# Containers      2         2         3.354kB   0B (0%)
# Local Volumes   1         1         209.5MB   0B (0%)
# Build Cache     0         0         0B        0B

# 删除镜像
docker rmi [-f] 镜像名称[:TAG]/ID [镜像2[:TAG]/ID]

## 全部删除
docker rmi [-f] $(docker images -aq)
```

## 虚悬镜像

仓库名、标签名都是`<none>`的镜像，俗称*虚悬镜像(dangling image)*

## 容器命令

```bash
# 启动容器
docker run [OPTIONS] 镜像 [COMMAND] [ARG...]
```

**OPTIONS**：

- `--name=容器名称`：为容器指定名称

- `-d`：后台运行容器，并返回容器ID

- `-i`：以交互模式运行容器，通常与`-t`同时使用

- `-t`：为容器重新分配一个伪输入终端，通常与`-i`同时使用

- `-P`：（大写）随机端口映射

- `-p`：（小写）指定端口映射

  | 参数                           | 说明                                |
  | ------------------------------ | ----------------------------------- |
  | `-p hostPort:containerPort`    | 端口映射，`-p 8080:80`              |
  | `-p ip:hostPort:containerPort` | 配置监听地址 `-p 127.0.0.1:8080:80` |
  | `-p ip::containerPort`         | 随机分配端口 `-p 127.0.0.1::80`     |
  | `-p hport:cport:udp`           | 指定协议 `-p 8080:80:tcp`           |
  | `-p 81:80 -p 443:443`          | 指定多个                            |

```bash
# 列出容器
docker ps [-a] [-q] [-l] [-n N]
```

- `-a`：列出所有容器
- `-l`：显示最近创建的容器
- `-n N` ：显示最近`N`个创建的容器
- `-q`：静默模式，只显示容器编号

```bash
# --- 容器内 ---

# 退出，容器停止
exit

# 退出，容器继续运行
# <Ctrl - p - q>
```

```bash
# 启动已停止的容器
docker start  容器ID/容器名

# 重启容器
docker restart  容器ID/容器名

# 停止容器
docker stop  容器ID/容器名

# 强制停止容器
docker kill  容器ID/容器名

# 删除容器
docker rm [-f] 容器ID/容器名

# 删除所有容器
docker rm -f $(docker ps -aq)
# 或
docker ps -aq | xargs docker rm

# 查看容器日志
docker logs  容器ID/容器名

# 查看容器内运行的进程
docker top  容器ID/容器名

# 查看容器内部细节
docker inspect  容器ID/容器名

# 进入正在运行的容器并以命令行交互
docker exec -it 容器ID bash
# 重新进入
docker attach 容器ID

# 拷贝文件
docker cp 容器ID:容器内路径 宿主机路径
docker cp 宿主机路径 容器ID:容器内路径

# 导出
docker export 容器ID > 文件名.tar
# 导入
cat 文件名.tar | docker import - 镜像用户/镜像名:TAG
```

`exec` 和 `attach` 的区别：

- `attach`直接进入容器启动命令的终端，不会启动新的进程，用 exit 退出会导致容器的停止
- `exec`是在容器中打开新的终端，并且可以启动新的进程，用 exit 退出不会导致容器停止

## 常用命令

```
attach    Attach to a running container                 # 当前 shell 下 attach 连接指定运行镜像 
build     Build an image from a Dockerfile              # 通过 Dockerfile 定制镜像 
commit    Create a new image from a container changes   # 提交当前容器为新的镜像 
cp        Copy files/folders from the containers filesystem to the host path   #从容器中拷贝指定文件或者目录到宿主机中 
create    Create a new container                        # 创建一个新的容器，同 run，但不启动容器 
diff      Inspect changes on a container's filesystem   # 查看 docker 容器变化 
events    Get real time events from the server          # 从 docker 服务获取容器实时事件 
exec      Run a command in an existing container        # 在已存在的容器上运行命令 
export    Stream the contents of a container as a tar archive   # 导出容器的内容流作为一个 tar 归档文件[对应 import ] 
history   Show the history of an image                  # 展示一个镜像形成历史 
images    List images                                   # 列出系统当前镜像 
import    Create a new filesystem image from the contents of a tarball # 从tar包中的内容创建一个新的文件系统映像[对应export] 
info      Display system-wide information               # 显示系统相关信息 
inspect   Return low-level information on a container   # 查看容器详细信息 
kill      Kill a running container                      # kill 指定 docker 容器 
load      Load an image from a tar archive              # 从一个 tar 包中加载一个镜像[对应 save] 
login     Register or Login to the docker registry server    # 注册或者登陆一个 docker 源服务器 
logout    Log out from a Docker registry server          # 从当前 Docker registry 退出 
logs      Fetch the logs of a container                 # 输出当前容器日志信息 
port      Lookup the public-facing port which is NAT-ed to PRIVATE_PORT    # 查看映射端口对应的容器内部源端口 
pause     Pause all processes within a container        # 暂停容器 
ps        List containers                               # 列出容器列表 
pull      Pull an image or a repository from the docker registry server   # 从docker镜像源服务器拉取指定镜像或者库镜像 
push      Push an image or a repository to the docker registry server    # 推送指定镜像或者库镜像至docker源服务器 
restart   Restart a running container                   # 重启运行的容器 
rm        Remove one or more containers                 # 移除一个或者多个容器 
rmi       Remove one or more images       # 移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除] 
run       Run a command in a new container              # 创建一个新的容器并运行一个命令 
save      Save an image to a tar archive                # 保存一个镜像为一个 tar 包[对应 load] 
search    Search for an image on the Docker Hub         # 在 docker hub 中搜索镜像 
start     Start a stopped containers                    # 启动容器 
stop      Stop a running containers                     # 停止容器 
tag       Tag an image into a repository                # 给源中镜像打标签 
top       Lookup the running processes of a container   # 查看容器中运行的进程信息 
unpause   Unpause a paused container                    # 取消暂停容器 
version   Show the docker version information           # 查看 docker 版本号 
wait      Block until a container stops, then print its exit code   # 截取容器停止时的退出状态值 
```


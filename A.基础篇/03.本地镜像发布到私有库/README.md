## 原理

使用官方 `registry` 镜像，可以搭建私有库。

- [docker hub](https://hub.docker.com/_/registry)
- [文档](https://docs.docker.com/registry/deploying/)

## 最简举例

```bash
# 启动私有仓库
docker run -d -p 5000:5000 --restart always --name registry registry:2

# 给本地镜像打仓库所要的tag
docker tag 919e2e812a95 localhost:5000/foo/vim

# 推送到仓库
docker push localhost:5000/foo/vim

# 删除本地镜像
docker rmi -f 919e2e812a95

# 从私有仓库拉取镜像
docker pull localhost:5000/foo/vim

# 运行
 docker run -it --rm localhost:5000/foo/vim vim
 
 # 使用CURL查看私有仓库有哪些镜像
 curl 127.0.0.1:5000/v2/_catalog
 
 
```


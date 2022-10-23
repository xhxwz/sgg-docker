## 命令

```bash
# 网络列表
docker network ls

# 创建网络
docker network create 网络名

# 删除网络
docker network rm 网络名
```

## 作用

- 容器间的互联和通信，以及端口映射
- 容器IP变动时，可以通过服务名直接网络通信，而不受影响

## 网络模式

| 模式        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| `bridge`    | 为每一个容器分配、设置IP等，并将容器连接到 `docker0`虚拟网桥，默认为该模式 |
| `host`      | 容器不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口 |
| `none`      | 容器有独立的 Network namespace，但并没有对其进行任何网络设置，如分配 veth pair 和网桥连接、IP等 |
| `container` | 新创建的容器不会创建自己的网卡和配置自己的IP，而是和一个指定的容器共享IP、端口范围等。 |

> `docker link` 已过时，不要再使用。
>
> 使用 `docker network create` 和 `docker run --network`替代。
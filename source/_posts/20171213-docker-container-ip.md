---
title: Docker容器网络
date: 2017-12-13 9:14:23
categories: knowledge
tags: [docker, network]
---
# Docker容器网络

linux下的docker默认创建网络：

> docker0, 172.17.0.0/16

可以将运行linux的宿主机看做路由器, 其上的docker0接口连接一台交换机与各容器相连:

```md
                +———————————+               +————+
        1.1.1.1 |R          |               | SW |       +—container1—+
public network interface    |               |  +----------172.17.0.11 |
                |           |172.17.0.1     |  | |       +————————————+
                |         docker0--------------+ |
                |           |               |  | |       +—container2—+
                +———————————+               |  +----------172.17.0.12 |
                                            |    |       +————————————+
                                            +————+  
```

详情可参照文档 [Docker Network Configuration Documentation](https://docs.docker.com/engine/userguide/networking/)

<!-- more -->

# 创建网络并为容器指定IP

## 自定义网络

和各虚拟机大厂网络类似, 可自定义4种模式, 默认bridge ~~从没用过另外3种~~

```bash
docker network create --subnet=192.168.1.0/24 [SubnetName]      # 创建
docker network ls                                               # 查看已创建的网络名称及模式
```

桥接网络可直接通过`ifconfig`查看其详情

## 容器IP

在`run`命令中加上`--network [SubnetName]`即可指定容器运行的网络. 同时, 需要指定IP的话, 此命令为必选项.  

```bash
docker run -i -t -v [/physical/site:/container/site] --network [SubnetName] --ip [X.X.X.X] [ImageName] bash
```

## inspect命令

`inspect`命令主要作用是将容器的各项属性通过json格式返回, 而其中包括了容器IP, 所以通常查看容器IP可以通过`inspect`的`--format`参数来看到:

```bash
$ docker inspect [ContainerID]
> [
    {
        ...
        "NetworkSettings":{
            ...
            "IPAddress":"172.17.0.2",
            ...
        }
        ...
    }
]

$ docker inspect -f '{{.NetworkSettings.IPAddress}}' [ContainerID]
> 172.17.0.2
```

但docker会映射容器名作为host条目，内部直接使用容器名即可访问其他容器，且通常会指定ip

# 注意事项

1. `--ip X.X.X.X`只能在用户定义网络上指定IP, 默认网络只能自动分配

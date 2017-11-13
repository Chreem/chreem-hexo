---
title: Docker Container IP
date: 2017-03-03 9:14:23
categories: cloud
tags: docker
---
# Default of Docker Network

Usually by `apt-get install docker.io` (~~or any other way~~) to get docker has it's own network config:

> docker0,172.17.0.0/16

Just like my Linux is a router & docker bridge is a switch:

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

<!-- more -->

So I meet the first pit.  
After I [baidu.com](https://www.baidu.com), should use `--ip X.X.X.X` assign container's IP, and I do that.The result is I ALWAYS young.Shell told me only user-defined network could assign IP.Because of static IP so that I can use Nginx High performence server & reverse proxy at the same time & convenience.

## Container IP

### Create Network

According to baidu, docker has 4 modes network (~~with my dingding thing~~), but who let me like to use bridge mode more than physical mode.

```bash
docker network create --subnet=192.168.100.0/24 SubnetName
```

then you can see a new bridge interface in config by using `$ ifconfig`

### Run container

The command of run container also should bring `--network SubnetName`, so that you can choose the network that you create

```bash
docker run -d -v /physical/site:/container/site --network SubnetName --ip X.X.X.X chreem/nginx
```

### Check Running Container IP

`inspect` is used to check the detail of container or other, the ip is included in this:

```bash
$ docker inspect ContainerID
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
```

And it also has `--format` param, `{.NetworkSettings.IPAddress}`  
*(Because of nunjucks' in hexo double brace syntax, I just use one instead)*

```bash
$ docker inspect -f '{{.NetworkSettings.IPAddress}}' ContainerID
> 172.17.0.2
```

## Other

Get more help from [Docker Network Configuration Documentation](https://docs.docker.com/engine/userguide/networking/)  
~~And why I use English. 再用鹦语剁手~~
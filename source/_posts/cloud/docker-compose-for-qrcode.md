---
title: docker-compose配置二维码登录环境
date: 2018-06-29 20:33:13
tags:
---
# docker-compose for QRCode Login

作为需要多个服务配合完成的二维码登录：  
每次到一个新环境都从零配置 ？f**k all files && delete all && run away : 爽到

本篇以自己写的二维码登录为例廖丕介绍一下[常见配置](https://github.com/Chreem/todo-list/blob/master/qrcode-login/docker-compose.yml)

## 所需部件

* `nginx` 为多个服务提供反向代理
* `redis` 暂存global login id及user login token
* `node` 提供登录及推送登录信息

## 参数说明

所有配置参数直接参考官方文档，某些会略微说明，不做赘述

1. docker volume

    每个容器创建时会同时创建数据卷，the same as created by docker-compose  
    不同的是单纯`docker-compose down`不会删除数据卷，需要加上`-v`参数才会连带删除数据卷  
    主动创建的外部卷依然得手动删除

2. multi bridge

    即使看起来是路由可达，bridge模式下的多个网桥之间依然不允许通信，某个容器需要同时访问最好靠加入两个网桥，即：

    ```yml
    networks:
      frontend:
        ipv4_address: 192.168.1.11
      backend:
        ipv4_address: 192.168.2.10
    ```

    设想：  
    例：ports参数使主机映射6379端口至redis容器所在的backend ipv4_address  
    因为主机映射的关系其他网桥如frontend gateway同样监听6379端口  
    所以frontend内的容器能通过gateway(此处192.168.1.1)连接backend的redis  
    
    问题：  
    并没测试过  
    多个没卵用的ip都会监听这个端口，只监听必要ip反而更麻烦  

3. expose

    美其名曰暴露端口，但实际上即使不加这条，其他容器同样能正常访问，主要功能应该是使用`docker ps -a`时能直观看到端口吧，emmmmm...

4. depends_on

    依赖服务，如本篇`auth_server`需要用到redis暂存sid&uid，需要redis先于他启动

5. build

    尽量不用build&Dockerfile构建，因为凭空多管理一个image反而成为累赘，不得不用build完成的复杂场景还是很少的  
    使用`docker-compose down --rmi local`能同时移除build参数构建的image

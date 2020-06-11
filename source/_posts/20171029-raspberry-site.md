---
title: raspberry-site
date: 2017-10-29 18:12:29
categories: daily
tags: [server, raspberry]
---
# Raspberry Site

目前作为自用web server以及硬件研究服务器, 用了各种功能

## 各种功能

1. 静态资源服务

    原计划用一个跑在docker容器中的node/express来做, 不过docker部署不太顺利, 暂时忽略

    apache2

2. GPIO Control

    之前用node fs象征性的写了写, 通过设置树莓派系统暴露出的/sys/gpio接口来控制  
    然而[别人家](https://github.com/jperkin/node-rpio)通过bcm2835的c文件直接对内存操作，其Readme也给出了性能对比，遂放弃自己造轮子

    rpio, express, socket.io

3. 上述的配套UI

    为了从公网可访问，需要建立一台websocket server，且node服务与前端都加入这个server

    socket.io-client

<!-- more -->

# 静态站点

## Apache2 CORS

不得不说apache的目录结构越来越像nginx了, 同样的xxx-available xxx-enable  
apache2的目录结构和1相比变化不小, 不过变化再大也不关我啥事, 反正我也用不上😹

### 步骤

```bash
# 据说header模块默认开启, 但我的没, 怕是用了假apache
> a2enmod headers
> vim /etc/apache2/apache2.conf

# 找到并将Header改为以下:
<Directory /var/www>
Options Indexes FollowSymLinks
AllowOverride All
Header set Access-Control-Allow-Origin "*"
Require all granted
</Directory>

# 检查配置文件语法并重启
> apachectl -t
> service apache2 reload
```

### 问题

不能使用类似express的`res.header('Access-Control-Allow-Origin', req.get('origin'))`来设置Origin，仅仅只能作为静态资源服务  

# GPIO接口控制

nodejs的gpio控制，目前唯一的作用是，开关灯😂

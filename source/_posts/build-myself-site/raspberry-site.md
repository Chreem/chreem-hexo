---
title: raspberry-site
date: 2017-10-29 18:12:29
categories: server
tags: site
---
# Raspberry Site

目前作为自用web server以及硬件研究服务器, 用了各种功能

## 各种功能

1. 静态资源服务

    原计划用一个跑在docker容器中的node/express来做, 不过docker部署不太顺利, 暂时忽略

    apache2

2. GPIO Control

    之前用node fs象征性的写了写, 通过设置树莓派系统暴露出的/sys/gpio接口来控制  
    然而在看了[别人家](https://github.com/jperkin/node-rpio)通过bcm2835的c文件直接对内存操作, 瞬间觉得自己就是个lowb... 其README.md也给出了测试效果。文件操作被内存完爆也在情理之中

    rpio, express, socket.io

3. 上述的配套UI

    由于是静态站点, 直接放在`/var/www/html`下了, 毕竟实际控制消息是靠自己vps上的`socket.io` server来传递的, 为了能从任何位置控制, 如果必须和派在一个内网岂不是很low  
    要zhuangb的话socket.io换成rabbitmq也完全OK, 不过站点也必须有express或其他后端支持才行, 暂时不考虑  

    > MQ不暴露在公网, 就算有访问控制  
    > --Chreem

    socket.io-client

<!-- more -->

## 部分配置

### Apache2 CORS

不得不说apache的目录结构越来越像nginx了, 同样的xxx-available xxx-enable  
apache2的目录结构和1相比变化不小, 不过变化再大也不关我啥事, 反正我也用不上😹

#### 步骤

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

#### 问题

但也有副作用, 不能像express通过`res.header('Access-Control-Allow-Origin', req.get('origin'))`的来设置Origin, 更不能直接拿数组当黑白名单, 这也是为何我只拿来放静态资源的原因之一. 而且apache2的`*`遇上session会比较蛋疼  
~~翻译成白话就是: 我不会...  反正也遇不上~~

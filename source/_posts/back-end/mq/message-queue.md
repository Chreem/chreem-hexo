---
title: message queue
date: 2017-10-24 18:11:43
categories: mq
tags: usage
---
# 假聊天室MQ

hello world用起来不就是聊天室么😂, 在消费者/订阅者几乎没耗时任务时确实很像.

## 作用

请求放在MQ中待后端空闲取走, ~~~~  
用以低耦合的模块合作  
应用交互对象由另一个应用改为MQ  

> 在单一服务器内部可通过多线程共享内存队列的方式实现异步  
> 分布式消息队列可以看作内存队列的分布式部署  
> --from 《大型网站技术架构 核心原理与案例分析》

~~又双叒叕一个银行排队, 做完了人/窗口 现在终于轮到取号了么~~  
~~没体验过那种规模的问题 实在很难体会具体用途~~  

<!-- more -->

## 模式

生产-消费显然是为了方便扩容, 后端宕机也能暂存消息  
发布-订阅

## 使用

均为windows下

### Redis

其中redis的订阅发布模式可直接作为一个简单的MQ使用.  
对于小数据的入队出队操作性能甚至超过RabbitMQ, 不过当数据接近10k Redis入队性能直线下降, 详情:  
<http://blog.csdn.net/sunxinhere/article/details/7968886>

1. 安装redis (win下安装完成后已自动运行其服务)
2. 命令`subscribe` `publish` `unsubscribe`:
    ```md
    cmd > redis-cli
    第一个客户端 127.0.0.1:6379> subscribe test
    第二个客户端 127.0.0.1:6379> publish test hehe
    ```

当然redis仅供尝试, 完全体还是得用纯正的MQ才行

### RabbitMQ

1. Erlang
2. 安装RabbitMQ
3. 允许远程用户
    ```bash
    # 本地连接默认通过guest用户, 而此用户仅localhost可用, 所以添加用户:
    rabbitmqctl add_user user password
    rabbitmqctl set_user_tags user administrator  
    rabbitmqctl set_permissions -p / user ".*" ".*" ".*"  
    ```
4. 防火墙允许TCP 5672

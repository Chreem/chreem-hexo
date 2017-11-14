---
title: Node basic usage
date: 2017-03-21 15:15:40
categories: node
tags: usage
---
# Node

简介如何安装及遇到过的问题

## Setup

1. Download from:  
    <https://nodejs.org/dist/v6.2.0/node-v6.2.0-linux-x64.tar.gz>

2. install:
    ```bash
    > tar -zxf node-v6.2.0-linux-x64.tar.gz
    # 或者
    > xz -d xxx.tar.xz
    > tar -xf xxx.tar

    # 1. 软连接
    > cd node-v6.2.0-linux-x64/bin
    > ln -s /home/chengm/node-v6.2.0-linux-x64/bin/node /usr/bin/node
    > ln -s /home/chengm/node-v6.2.0-linux-x64/bin/npm /usr/bin/npm

    # 2. 或修改环境变量
    > vi /etc/profile.d/node_env.sh
    #!/bin/sh --this-shebang-is-just-here-to-inform-shellcheck--
    export NODE_HOME=/usr/local/node/node-v7.3.0-linux-x64/bin
    export PATH=$NODE_HOME:$PATH
    > source /etc/profile

    # 注: 两种方式视情况使用
    ```

3. 运行测试：
    ```bash
    root@Beryllium:~# node
    > console.log("hello world!");
    hello world!
    undefined

    root@Beryllium:~/NodeCode# cat helloworld.js
    console.log("hello world");
    root@Beryllium:~/NodeCode# node helloworld.js
    hello world
    ```

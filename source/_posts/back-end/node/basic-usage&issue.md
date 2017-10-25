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
    root@xxx# tar -zxf node-v6.2.0-linux-x64.tar.gz

    Or root@xxx# xz -d xxx.tar.xz
    root@xxx# tar -xf xxx.tar

    root@xxx# cd node-v6.2.0-linux-x64/bin
    root@xxx:/home/chengm/node-v6.2.0-linux-x64/bin# ln -s node /usr/bin/node
    root@xxx:/home/chengm/node-v6.2.0-linux-x64/bin# ln -s npm /usr/bin/npm
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

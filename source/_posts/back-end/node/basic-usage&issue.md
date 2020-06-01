---
title: Node basic usage
date: 2017-03-21 15:15:40
categories: node
tags: usage
---
# Node

1. 简介node.js的安装
2. `package.json`中的某些配置
3. 依赖打包发布至npm
4. 过程中的问题及注意事项

<!-- more -->

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

## NPM

### `package.json`部分配置

新项目目录下通常使用`npm init`来创建，而非手动新建。常用属性不做赘述，仅在需要发布时需要留意的属性如下：

```js
"bin": { "chr": "./index.js" }, // 作为命令执行的名字及路径
"preferGlobal": true,           // 是否需要作为工具，全局安装

"scripts": {                    // npm run命令对应的执行语句，可将命令行依赖写入如下脚本中执行，即可只安装于本目录下，而非全局安装
    "test": "mocha"             // 如start,test等命令可省略run
},

"repository": {                 // 该包的仓库类型及链接
    "type": "git",
    "url": "https://github.com/Chreem/chr-mock"
},
```

### publish

将写好的包发布至npm，~~用以污染npm池，拉低包素质~~

`npm login`后默认会将登录token存在`~/.npmrc`中，  
随后便可执行`npm publish`来发布

## 问题

### npm命令在安装某些跨平台依赖时 出现permission denied

`npm i * --unsafe-perm`

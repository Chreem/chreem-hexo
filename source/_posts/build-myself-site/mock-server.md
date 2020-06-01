---
title: nodejs mock-server
date: 2017-03-17 23:53:18
categories: server
tags: mock
---
# Build Nodejs based Mock-Server

图省事用的express，数据就用*.json保存，假装自己有个mongodb。  
同时测试npm发布：  
[![chr-mock](https://img.shields.io/npm/v/chr-mock.svg?style=flat-square)](https://www.npmjs.com/package/chr-mock) [![Travis](https://img.shields.io/travis/Chreem/chr-mock.svg?style=flat-square)](https://travis-ci.org/Chreem/chr-mock) [![npm](https://img.shields.io/npm/l/chr-mock.svg?style=flat-square)](https://www.npmjs.com/package/chr-mock)

## environment

### package.json

自定义命令需要在package.json里修改bin属性，同样因为npm命令行，还需要全局安装提示：

```js
    "preferGlobal": true,
    "bin": {
        "chr": "./index.js"
    },
```

或者装好后添加至全局：

```bash
$ cd chr-mock
~/chr-mock$ npm link
```

<!-- more -->

### [commander](https://www.npmjs.com/package/commander)

node命令行用  
能很方便的配置各类参数以及自动生成帮助文档--help  

```js
#!/usr/bin/env node
// 见的比较多的应该是#!/bin/bash，此处同理

const program = require('commander');
var filename;

program
    .command('watch [file]')
    .description('Add a file to watch')
    .action(file => {
        filename = file;
    });
program         // 之所以单列一条而没和上面放一起是为了能正常显示帮助文档
    .option('-p, --port <n>', 'Add port to listen, default is 8080', /^[1-9]\d*$/, 8080);
program.parse(process.argv);

if (typeof filename === 'undefined') {
    program.outputHelp();
    process.exit(1);
}
/* 参数不对显示帮助文档
E:\Web\node\chr-mock>node index.js watch

  Usage: index [options] [command]


  Commands:

    watch [file]  Add a file to watch

  Options:

    -h, --help      output usage information
    -p, --port <n>  Add port to listen, default is 8080
*/

console.log('file:' + filename);
console.log('port:' + program.port);
```

### [express](http://www.expressjs.com.cn/)

后面要做的基本上就在这里的watch.js里写了  
根据上面命令行拿到的文件及监听端口运行express  

```js
/* index.js */
const watch = require('./watch.js');
... //上一步
watch.run(filename, program.port);

/* watch.js */
const express = require('express')
    , app = express();
module.exports = {
    run(file, port){
        app.get('/', (req, res)=>{
            res.send('hello~');
        });
        app.listen(port);
    }
};
```

以及各种中间件：  

1. 跨域：cors
2. 解析post body：body-parser

其他：

1. url
2. path
3. jsonfile

## analysis

### router match

首先要做的就是根据路由来匹配访问字段，没啥疑问应该  
该节参考[api 路由](http://www.expressjs.com.cn/guide/routing.html)  
数据深度不定，无法使用`express.Router`，需通过`url.parse(*).pathname`自行解析

```js
run(file, port){
    app.all('*',(req, res)=>{
        let params = url.parse(req.originalUrl).pathname
            .replace(/^\//,'')
            .replace(/\/$/,'')
            .split('/');

        // 参数加工处理   及各种判断
        // 请求分类     以及各种判断
        // 返回值
    })
    app.listen(port);
}
```

## publish to npm

### 手动

只需要简单几步就能完成……：  

```bash
# 1. 跳过第二步
# 2. 注册
$ cd chr-mock             # 至项目目录
~/chr-mock$ npm login     # 登录
~/chr-mock$ npm publish   # 发布
```

然后就能从公有仓库下载：

```bash
npm i -g chr-mock
chr watch data.json
```

### automatically

1. [@baidu.com](https://www.baidu.com) & 跳过第此后所有
2. github & travis ci配置
3. 查看完整的login token, 通常在`~/.npmrc`里
4. Travis CI配置环境变量存入token
5. .travis.yml添加deploy信息：

    ```yaml
    deploy:
        provider: npm
        email: $npm_login_email
        api_key: $npm_token_key
        on:
            branch: master
    ```

6. git push

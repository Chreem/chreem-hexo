---
title: how to deal data
date: 2017-03-17 09:42:33
tags: [axios,rest]
categories: site
---
# Data

## mock server

简单几步就能完成：

```md
1. 拿node/nginx或者别的啥来自己写个后端RESTful实现  这里就拿node举栗了
2. npm上找个json的操作库，然后抄段增删改查
3. express或者别的来响应请求
4. 运行发布
```

搭建步骤  移步["nodejs mock-server"](https://hexo.chreem.xyz/2017/03/18/site/MockServer/)

## directory for now

```md
site                            //站点根目录
├── lib                         //各种数据处理
│   ├── data-structure          //如名。。。除了提供基本数据结构外还有各种转换
│   │   └── index.js                //应该是和数据沾边的都会在这
│   └── service                 //后端数据交互
│       ├── index.js            //提供请求方法，默认由后端获取
```

### /lib/service/index.js

1. get  
    说多少不如贴段代码  
    顺带……格式用qs调整纯属偷懒
    ```js
    import axios...
        , qs...;
    let SITE_URL = 'http://127.0.0.1:8080/';
    /**
      * 获取数据，详情参照chr-mock
      * @returns {{}} 所需数据
      */
    getData(url = '', params = {}){
        let data = '';
        if (params.toString() !== '{}') {
            data = '?' + qs.stringify(params);
        }
        return axios.get(SITE_URL + url + data)
            .then(res => { return res.data; })
            .catch(error => {
                console.error(error);
                return {};
            });
    }
    ```
2. post/patch 差不多不多说
3. delete 详情移步mock server

## ~~old~~

目前已移步mock server  
把数据获取和处理单列一项，据说能弱耦合（~~神特喵弱，都是自己用的~~），目录如下：

```md
site                            //站点根目录
├── lib                         //各种数据处理
│   ├── data-structure          //如名。。。除了提供基本数据结构外还有各种转换
│   │   └── index.js                //应该是和数据沾边的都会在这
│   └── service                 //后端数据交互及本地模拟
│       ├── backend.js                  //*退役       从后端获取
│       ├── data.test.json              //*退役       本地模拟数据
│       ├── index.js            //提供请求方法，默认由后端获取
│       └── test.js                     //*退役       从本地获取  
```

<!-- more -->

### require('./lib/service')

其中默认方法：`mode='backend'`，可选本地测试：test，以及目前创建的方法：

```js
const Service = require('./lib/service');
Service.setMode('test');                //*退役   配置数据获取途径

Service.getData();                  //获取所有已知数据
Service.getDataRange(start,end);    //限制获取范围，类似MySQL的limit
Service.getDataByID(id);            //如名
Service.getLength();                //获取所有数据个数而非详情
                                    // TODO 把上面的各种getData想办法揉到一起
```

#### backend

~~还没开始倒腾后端，暂且无视~~  胎死腹中

#### test

* new  
    今天17.3.17刚知道这玩意可以自建mock server来做，所以test.js不久就能退休了
* old  
    犹豫半天决定test.js里还是不写和数据本身相关的东西，只把读到的原样返回

---
title: Vue SSR stack
date: 2017-03-14 09:55:25
categories: server
tags: vue
---
# node module list

a list about node modules that were used

## global

* vue-cli   主要用于单组件调试而非生成项目

## running

* vue
* vue-router
* moment.js 处理时间

## server

* express   nothing to say
* rimraf    便利的删除文件夹工具
* cross-env   提供不同系统的相同命令支持
* webpack-hot-middleware  
    无需webpack-dev-server也能hot-reload  
* webpack-dev-middleware  
* serve-favicon favicon没啥可解释的
* compression   提供诸如gzip压缩功能
* vue-server-renderer  
    vue的ssr支持,第二个参数可选传入一个对象并包含例如template,cache等:  
    ```js
    const vueSSR = require('vue-server-renderer')
        renderer = vueSSR.createBundleRenerer(bundle,{
            template:fs.readFileSync('....html'),
            cache:require('lru-cache')({
                max:...,
                maxAge:...
            })
        });

    ```
* lru-cache 看名字就知道用来缓存的,用于vue-server-renderer的各个create函数create*Renerer()的第二个参数,设置缓存对象
* vuex-router-sync  将vue-router信息注入vuex
* memory-fs 如名

## build

* path
* webpack
* autoprefixer  自动完善css的浏览器支持
* html-webpack-plugin   补全根据hash生成的js文件至模板
* vue-ssr-webpack-plugin    如名
* sw-precache-webpack-plugin    service worker支持

## other

* blueimp  用于webpack插件html-webpack-plugin的模板中
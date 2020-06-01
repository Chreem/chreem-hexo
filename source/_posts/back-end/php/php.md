---
title: PHP
date: 2017-03-01
categories: php
tags: usage
---
# php 相关环境

windows上的nginx与php fastcgi.  
首先,如果仅为了调试方便而且正好用了vscode,完全可以只用php+code插件[PHP Server](https://marketplace.visualstudio.com/items?itemName=brapifra.phpserver)  
~但毕竟生命在于折腾~ 也没多麻烦  
安装依赖管理
code格式化插件

<!-- more -->

## FastCGI

### Nginx

1. 直接下载
2. 取消nginx.conf中php fastcgi部分的注释
3. cmd > start nginx

### php-cgi.exe

1. fastcgi使用non-thread safe,[详情此处](https://stackoverflow.com/questions/1623914/what-is-thread-safe-or-non-thread-safe-in-php)
2. cmd > start php-cgi.exe -b 127.0.0.1:9000

## Composer

参照npm来的php依赖管理

1. 将php目录添加进环境变量path
2. 开启php openssl模块:
    * 去掉php.ini的openssl模块
    * 添加插件目录`extension_dir="***/ext"`
3. 安装[composer](https://pkg.phpcomposer.com/#how-to-install-composer),运行完毕后会在相应目录下载composer.phar
4. 【windows下】在安装composer.phar的目录新建composer.bat,内容:

    ```bat
    @php "%~dp0composer.phar" %*
    ```

5. [可选]修改composer源

    ```cmd
    > composer config -g repo.packagist composer https://packagist.phpcomposer.com
    ```

## 代码格式化

插件:[PHP IntelliSense](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-intellisense)
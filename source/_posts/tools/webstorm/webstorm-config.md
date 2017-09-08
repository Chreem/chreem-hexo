---
title: webstorm-config
date: 2017-03-14 16:25:32
tags: [webstorm,settings]
categories: 环境
---
# Base

平时用的相当多的除了vscode就是webstorm了，当然不仅限于webstorm  
本篇主要还是积累一些平时用上用不上的settings修改，毕竟把工具调教顺手还是相当有必要的

## ~~vue extension~~

2017.1版已加入vue支持.  

### old

vue扩展需另行安装（~~明明angular react都直接有了，就不能再厚道一点？~~）

1. 安装
    File-->Settings-->Plugins-->Install JetBrains plugins  
    搜vue.js，install，reboot webstorm……OK ~~好了，关机睡觉。。。~~
2. 配置Vue模板
    依然settings，-->Editor-->File Types-->Vue.js Template的Registered Patterns里是否包含了*.vue  
    若没有加上即可  
    然后在-->Editor-->File and Code Templates下添加Vue File，具体如下：  
    ![image](https://cloud.githubusercontent.com/assets/12951147/23892069/19d57a8a-08d4-11e7-8690-3ec3a9c83be8.png)
3. 安心食用

<!-- more -->

## css排版

webstorm默认的css排版会把所有属性拆开单成一行，如果我只有一条。。。就强行被纵向加深（~~可以假装自己写了三倍行数的代码~~）

```css
.hidden-xs, .hidden-lg {
    min-width: 10vw;
}

@media (min-width: 769px) {
    .hidden-lg {
        display: none;
    }
}

@media (max-width: 768px) {
    .hidden-xs {
        display: none;
    }
}
```

所以File-->Settings-->Editor-->Code Style-->CSS-->Other，勾上Keep single-line blocks
至少{}里的可以改成这样了：  
可选保持上面的格式，选择余地略多一点而已

```css
.hidden-xs { min-width: 10vw; }     /* 可以写在一行 */

@media (min-width: 769px) {
    .hidden-lg {                    /* 依然可以换行 */
        display: none;
    }
}

/* ... 默认设置下选择器间会空一行 */
```

当然觉得每条之间多空的一行很碍眼还能继续缩减  
File-->Settings-->Editor-->Code Style-->CSS-->Other，把Blank lines between blocks改成0就OK了：  

```css
.hidden-xs, .hidden-lg { min-width: 10vw; }
@media (min-width: 769px) {
    .hidden-lg { display: none; }
}
@media (max-width: 768px) {
    .hidden-xs { display: none; }
}
```
---
title: webstorm-config
date: 2017-03-14 16:25:32
tags: [webstorm,settings]
categories: 环境
---
# Base

平时用的相当多的除了vscode就是webstorm了，当然不仅限于webstorm  
本篇主要还是积累一些平时用上用不上的settings修改，毕竟把工具调教顺手还是相当有必要的

|主要内容|
|---|
|修改CSS排版|
|关闭拼写检查|

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

## 关闭拼写检查

File-->Settings-->Editor-->Inspections-->Spelling 去掉此处的√即可

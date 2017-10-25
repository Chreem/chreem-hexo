---
title: bootstrap
date: 2017-03-14 23:05:26
categories: css
tags: bootstrap
---
# 实现

主要是栅格系统的实现  
话不多说上源码……的less版

```less
.container {
    margin-left  : auto;
    margin-right : auto;
    padding-left : 15px;
    padding-right: 15px;
    @media (min-width: 768px)  { width: 750px;  }
    @media (min-width: 992px)  { width: 970px;  }
    @media (min-width: 1200px) { width: 1170px; }
}
```

1. margin左右分别配置auto不用多说，padding左右是为了防止内容触碰浏览器边界，也为了之后.row和.col嵌套，若想内部元素也有一定的间隔就无需一个个的设置
2. 看媒体查询可以发现container的四个档位，分别是指定平板、大屏、超大屏的三个固定值，剩下的就是小屏的不定宽了。
3. 指定width比屏幕略窄则是为了防止元素过高出现垂直滚动条时压缩.container导致其出现水平滚动条

<!-- more -->

## tips

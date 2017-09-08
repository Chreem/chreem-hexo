---
title: less
date: 2017-03-22 10:09:20
tags: [less,webpack]
categories: css
---
# less syntax carding

## base syntax

### Less 和 Sass 中的嵌套

引用自<http://www.runoob.com/bootstrap/bootstrap-css-codeguide-html.html>

> 避免非必要的嵌套。这是因为虽然你可以使用嵌套，但是并不意味着应该使用嵌套。只有在必须将样式限制在父元素内（也就是后代选择器），并且存在多个需要嵌套的元素时才使用嵌套。  

```less
// Without nesting
.table > thead > tr > th { … }
.table > thead > tr > td { … }

// With nesting
.table > thead > tr {
    > th { … }
    > td { … }
}
```

<!-- more -->

## how to use in webpack

webpack2:

```js
    rules:[
        ...
        {test: /\.less$/,loader:'style-loader!css-loader!less-loader'},
        ...
    ]
```

## trouble

### webstorm中的 *.vue style 使用less/scss

如果啥都不写，显然会有恶心的红色波浪线，但是又能跑起来，而且自动格式化用不了就觉着很难受  
解决办法也很简单，用`rel="stylesheet/less"`告诉webstorm即可：

```html
<template>...</template>
<style rel="stylesheet/less" lang="less">
    .insideElement(@left:10vw) {
        margin-left:@left;
        ...
    }
    .class {
        .insideElement(40px);
        ...
    }
    /* ... */
</style>
<script>...</script>
```

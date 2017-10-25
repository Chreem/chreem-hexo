---
title: css-flex
date: 2017-03-09 12:57:31
categories: css
tags: flex
---
# accumulation

## 紧贴页面底部的页脚  

使用flex会相当容易：

```html
<!-- index.html -->
...
<head>
    <style>
        /*...*/
        body{
            display:flex;
            flex-flow:column;
            min-height:100vh;
        }
        .content{ flex:1; }
    </style>
</head>
<body>
    <div class="content">content</div>
    <footer>footer</footer>
</body>
...
```

<!-- more -->

## 垂直居中

1. Round 1  
    父元素使用flex布局，只需要多一句：
    ```css
    .parent{
        display:flex;
        align-items:center;
        ...
    }

    .child{
        ...
    }
    ```
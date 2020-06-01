---
title: Jetbrains家族配置（主要为webstorm）
date: 2017-03-14 16:25:32
categories: daily
tags: ['software', 'webstorm']
---
# Jetbrains家族配置

在最新版加入自动同步配置功能后，本篇越发用不上，纯属备忘

* 字体，字号
* 整行上下移动快捷键
* css排版
* 拼写检查

<!-- more -->

## css排版

关闭单行换行、隔行等

* 初始状态：

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

* 调整：

    File-->Settings-->Editor-->Code Style-->CSS-->Other
    
    勾上Keep single-line blocks，单行换行  
    Blank lines between blocks改成0，代码块之间不空行

* 调整后

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

当初很介意绿色波浪线，现在佛了，也就不关了

File-->Settings-->Editor-->Inspections-->Spelling 去掉此处的√即可

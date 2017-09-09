---
title: Linux basic usage
date: 2017-08-09 21:48:27
tags: System
---
# how to use

## 常用命令

|分类|作用|命令|示例|
|---|---|---|---|
|系统信息|查看CPU信息|`cat /proc/cpuinfo`||
|---|---|---|---|
|用户权限|加入组|`usermod -a -G group user`||
||新建用户时指定组|`useradd -g group user`||

<!-- more -->

## issue

1. Too many levels of symbolic links
    使用绝对路径代替相对路径建立
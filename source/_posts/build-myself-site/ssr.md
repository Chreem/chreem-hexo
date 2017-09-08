---
title: Vue Server-Site Render
date: 2017-04-03 10:02:29
tags: [vue,ssr]
categories: site
---
# Vue的SSR

初试SSR,真的搭起来也就不觉得多麻烦了,最终是靠express的res.send(xxx);而xxx则靠vue-server-renderer的createRenderer来从模板或Vue实例获取的一串字符串.今后说不定就改成Koa或Egg了.~~(生命不息折腾不止)~~


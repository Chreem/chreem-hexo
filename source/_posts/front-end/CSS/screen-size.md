---
title: screen-size
date: 2017-03-14 09:59:51
tags: media
categories: css
---
# @media

在此之前我只知道bootstrap……然而不知为何最近丝毫不想用这玩意  
~~这套全家桶并不如Vue来的有快感~~ 在自己有足够水平写组件库之前还是安心拿来主义好了，比如最近就用的饿了么Element UI

## 媒体查询

### 跳坑爬坑之旅  

还是跳了个坑=，=，之前百度到的这段里
`@media screen and(min-width: 1000px)`如果单独放到一个css文件然后从其他地方引入`@import './screen.css'`会导致媒体查询不能正常工作。小小修改一下即可：

```css
    .screen {
        display: flex;
        background-color: white;
    }

    @media (min-width:  768px) { .screen { width: 750px; } }
```

<!-- more -->

### ~~@media screen 根据不同尺寸屏幕决定不同样式~~

实际使用如下：

```html
<style>
    .screen {
        display: flex;
        background-color: white;
    }

    /* 即屏幕尺寸＞1000时.screen宽度为50vw，1000≥...≥701的宽度40vw，700的咋的咋的 */
    @media screen and(min-width: 1000px) { .screen { width: 50vw; } }
    @media screen and(max-width: 1000px) { .screen { width: 40vw; } }
    @media screen and(max-width:  700px) { .screen { width: 20vw; } }
</style>
```

需要注意的是@media的顺序，即从上到下判断  
~~至于网传的js实现……能用css做就不去想js~~，说不定真用上了呢，反正我不会  
但获取尺寸还是有必要知道的，可能今后根据不同屏幕lazy load不同样式用得上吧。。。茫茫多的宽高：

```js
/* 网页可见区域宽：*/document.body.clientWidth
/* 网页可见区域高：*/document.body.clientHeight
/* 网页可见区域宽 (包括边线的宽) ：*/document.body.offsetWidth
/* 网页可见区域高 (包括边线的宽) ：*/document.body.offsetHeight
/* 网页正文全文宽：*/document.body.scrollWidth
/* 网页正文全文高：*/document.body.scrollHeight
/* 网页被卷去的高：*/document.body.scrollTop
/* 网页被卷去的左：*/document.body.scrollLeft
/* 网页正文部分上：*/window.screenTop
/* 网页正文部分左：*/window.screenLeft
/* 屏幕分辨率的高：*/window.screen.height
/* 屏幕分辨率的宽：*/window.screen.width
/* 屏幕可用工作区高度：*/window.screen.availHeight
/* 屏幕可用工作区宽度：*/window.screen.availWidth
```
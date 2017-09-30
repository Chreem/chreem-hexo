---
title: js issue
date: 2017-09-25 17:03:43
tags:
---
# js问题合集

## URL.createObjectURL

目前是实验性功能, 浏览器仅提供部分支持  
使用`FileReader.readAsDataURL`代替:  

```js
function readURL(file){
    return new Promise(res => {
        reader.onload = function () { res(this.result); }
        reader.readAsDataURL(file);
    })
}
```

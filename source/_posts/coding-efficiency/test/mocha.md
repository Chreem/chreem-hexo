---
title: mocha
date: 2017-03-22 10:00:14
tags: [test,mocha]
categories: tools
---
# Mocha test tools

## trouble

### 超时

Mocha提供了两种异步测试方式，分别是done();和返回promise  

```js
/* done(); */
it('test', done => {
    /* something tasks */
    done();
});

/* promise */
it('promise', ()=>{
    return fetch().then()...
})
```

而出现使用promise超时若不是后端问题十有八九就是把promise放到`done=>{}`里去了

```js
/* 酱紫怎么调超时时间都没卵用，因为必须要见到done();而done又与promise互斥 */
it('error', done=>{
    return fetch()
        .then()
        .catch(); //<---wrong
})
```

### 捕获failing

Mock的failing通过promise的.catch的异常捕获，所以如果自己强行写了个`.catch`就会导致结果passing  

```js
/* wrong */
it('error', ()=>{
    return fetch().then()...catch()
});
```
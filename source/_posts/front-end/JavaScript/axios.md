---
title: axios
date: 2017-03-22 10:33:15
tags: [axios,ajax]
categories: js
---
# axios

## trouble

### axios delete don't have request.body

根据文档说明：

> `data` is the data to be sent as the request body. Only applicable for request methods 'PUT', 'POST', and 'PATCH'. When no `transformRequest` is set, must be of one of the following types:
> - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
> - Browser only: FormData, File, Blob
> - Node only: Stream

所以为了图懒-，-  必须指定数据放在data里，而且为了提供提交表单处理支持，添加头部信息

```js
// not for delete
axios.put(url, someObj).then();

// for Content-Type:application/x-www-form-urlencoded
const qs = require('qs');
axios.put(url, qs.stringify(someObj)).then();

axios.defaults.headers.delete['Content-Type'] = 'application/x-www-form-urlencoded';
axios.delete(url, {
    data:someObj
}).then();
```
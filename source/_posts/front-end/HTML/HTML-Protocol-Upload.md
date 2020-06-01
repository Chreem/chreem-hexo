---
title: HTML Protocol Authorization&Upload
date: 2017-09-21 17:57:04
categories: html
tags: protocol
---
# html协议认证及上传相关字段

站点登录认证可略微参考<https://zhuanlan.zhihu.com/p/29101305>  
目前自己正在使用的是貌似token的一套: 发起登录请求并通过验证后顺带传回客户端一坨字符串, 此后的各种请求中需将那一坨字符串附在Authorization头中, 匹配才进行后续操作.

涉及字段:  

|名称|Value|
|---|---|
|Access-Control-Allow-Credentials|true|
|Access-Control-Allow-Headers|Accept, Authorization, Content-Type, X-Requested-With, Range|
|Access-Control-Allow-Methods|GET,POST,PUT,PATCH,DELETE|
|Access-Control-Allow-Origin|`http://localhost:8080`|
|Authorization|xxx...|
|Cookie|xxx...|
|Content-Length|xxx...|
|Content-Type|application/json|
|Server|chr/0.0.1|
|X-Powered-By|Chreem|

<!--more-->

## 协议本身

1. HTTP/1.1

  持久连接Connection: keep-alive  
  管道机制: 多个资源复用一条TCP连接, 但一条连接中的资源顺序加载  
  对应上条而使用Content-Length字段  
  分块传输编码及Transfer-Encoding: chunked字段  
  GET,POST外的如PUT,PATCH,DELETE等字段

2. WebSocket

  通过HTTP/1.1握手, 相比HTTP/2的服务端推送更偏向于即时通讯

3. HTTP/2

  服务端推送, 相比HTTP/1.1客户端解析HTML再发出静态资源请求, HTTP/2可将HTML返回并主动推送静态资源

## 认证

为了配合后端验证, 保存用户加入的聊天室及权限以免每次请求都得连库验证, 依然使用了express-session, 需要客户端发送Cookie  

服务端需提供CORS头, 为设置Cookie需 Credentials设置为true且 Origin不能为*

```js
//...
HEADERS: {
    'Access-Control-Allow-Credentials': 'true',
    'Access-Control-Allow-Methods': 'GET,POST,PUT,PATCH,DELETE',
    'Access-Control-Allow-Headers': 'Accept, Authorization, Content-Type, X-Requested-With, Range',
    'Access-Control-Expose-Headers': 'Content-Length',
    'X-Powered-By': 'Chreem <chreem@qq.com>',
    'Server': 'chr-server/0.0.1'
},

app.use((req, res, next) => {
    res.header(HEADERS);
    res.header('Access-Control-Allow-Origin', req.get('origin')) // DEBUG_MODE
    return req.method === 'OPTIONS' ? res.sendStatus(200) : next();
});
```

客户端还需允许验证头withCredentials

```js
// 栗子axios
axios.defaults.withCredentials = true;
```

至于token, 虽然存在Cookie中了, localStorage更常用

## 上传

文件上传似乎更多的是POST Content-Type: 'multipart/form-data'  
自行组装请求体直接使用原生FormData可省去诸多麻烦:  

```js
function uploadFile({fileName:String, file:File, token:String}) {
    if (site.DEBUG_MODE) {return token}
    let formData = new FormData();
    formData.set('key', fileName);
    formData.set('token', token);
    formData.set('file', file, fileName);

    let xhr = new XMLHttpRequest();
    xhr.open('post', QINIU_UPLOAD_DOMAIN, true);
    xhr.send(formData)
}
```

最终req.body形如:

```js
POST http://upload.xxx/
Content-Type: multipart/form-data; boundary=<Boundary>
--<Boundary>
Content-Disposition: form-data; name="key"

<resource_key>
--<Boundary>
Content-Disposition: form-data; name="token"

<upload_token>
--<Boundary>
Content-Disposition: form-data; name="file"; filename="[文件名]"
Content-Type: <MimeType>

[文件内容]
--<Boundary>--
```

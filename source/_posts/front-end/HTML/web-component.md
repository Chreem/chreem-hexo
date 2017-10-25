---
title: web-component
date: 2017-09-30 17:44:40
categories: html
tags: component
---
# WebComponent

原生组件写法  
link:rel=import  
currentScript.ownerDocument  
css作用域  
shadowRoot  

<!--more-->

## startup

新建目录及文件:  

```md
web-component
        |
        -index.html
        -hello.js
```

```html
<!-- index.html -->
<body>
    <hello-component></hello-component>
    <script src='hello.js'></script>
</body>

<!-- hello.js  (markdown语法高亮所需而加上的script标签) -->
<script>
class Hello extends HTMLElement {
    createdCallback() {
        var h1 = document.createElement('h1');
        h1.innerText='test'
        this.appendChild(h1);
    }
}
document.registerElement('hello-component', Hello);
</script>
```

不过很明显, 以上写法只适用于dom结构简单的组件. 如果是特效大件, 更偏爱直接用html标签的写法来做, 于是有后文

## link:rel=import

将hello.js改为hello.html并在index中的link标签加载  

* link标签加载的html script会执行, 但dom不渲染
* 加载的document为index的全局document

```html
<!-- index.html -->
<head>
    <link rel="import" href="./hello.html">
</head>
<body>
    <hello-component></hello-component>
</body>
```

## css作用域等

* 通过shadowRoot可以让组件内的样式不受外部全局css影响
* document.currentScript.ownerDocument获取当前文档上下文
* importNode拷贝节点 (以便多次使用组件)

hello.html

```html
<body>
    <template id="hello">
        <style>
            h1{ color:red }
        </style>
        <h1>test</h1>
    </template>

    <script>
        const indexDoc = document;
        const helloDoc = document.currentScript.ownerDocument;
        const hello = helloDoc.querySelector('#hello');

        class Hello extends HTMLElement {
            createdCallback() {
                let shadow = this.createShadowRoot();
                shadow.appendChild(indexDoc.importNode(hello.content, true));
            }
        }
        indexDoc.registerElement('hello-component', Hello)
    </script>
</body>
```

index.html

```html
<head>
    <style>
        h1 { color: blue }
    </style>
</head>

<body>
    <hello-component></hello-component>
    <h1>test</h1>
    <!-- <x-product data-img="http://localhost:4000/head.jpg"></x-product>
    <script src="hello.js"></script> -->
</body>
```

效果:

![组件](/images/web-component1.png)
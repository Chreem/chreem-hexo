---
title: vue's accumulation
date: 2017-03-07 21:14:01
categories: js
tags: vue
---
# vue family

It's accumulation for vue.  
[![Vue version](https://img.shields.io/npm/v/vue.svg)](https://vuejs.org/)  
Rather than accumulation, problems summary is more likely.  
All of these are running by `npm install`, makesure npm&node was installed.

<!-- more -->

## some arounds

### vue-cli

* simgle vue file debug  
    ```bash
    # vue-cli 2.8+
    vue build app.vue
    ```

### vue-loader

* delete space between every elements
    ```js
    {
        vue:{
            preserveWhitespace: false
        }
    }
    ```

## trouble

### Dev-Tools插件无法使用

chrome的vue调试插件无反应,不出现  
改下配色模式即可,dark<->light

### vue runtime-only

* Recurrence  
    控制台虚区：`index.bundle.js:1876 [Vue warn]: You are using the runtime-only build of Vue where the template compiler is not available. Either pre-compile the templates into render functions, or use the compiler-included build. (found in <Root>)`
    ```js
    // ./router.js
    // ...(omit import&vue.use)
    const indexPage = require('./index/');
    const router = new VueRouter({
        routes: [
            {path: "/index", alias: "/", component: indexPage}
        ]
    });
    export default router;

    // ./index/index.js
    module.exports = {
        name: "index",
        template: require('./index.html'),
        style: require('./index.css'),
        data(){
            return {
                test: "qweasd"
            }
        }
    };
    ```
* Reason  
    `import Vue from 'vue'`默认打包/node_modules/vue/dist/vue.runtime.common.js，即runtime-only不带模板编译器，而目录结构中index.js使用了`template`属性，

    From [Vuejs.org](https://vuejs.org/v2/guide/installation.html#Runtime-Compiler-vs-Runtime-only)

    > Runtime + Compiler vs. Runtime-only  
    > If you need to compile templates on the fly (e.g. passing a string to the template option, or mounting to an element using its in-DOM HTML as the template), you will need the compiler and thus the full build:

* solve  
    there's two ways to solve this problem:
    1. 页面内容不多，可将index.js,index.html,index.css直接塞进index.vue中，用vue-loader即可  
        继续使用runtime-only mode：
        ```js
        // ./router.js
        ...
        const indexPage = require('./index/index.vue');
        ...

        // ./index/index.vue
        ... (normal vue file)
        ```

    2. runtime + compiler  
        修改webpack配置，`import`默认引入改为vue.js而非vue.runtime.js
        ```js
        // webpack.config.js
        module.exports = {
            // ...
            resolve: {
                alias: {
                'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' for webpack 1
                }
            }
        }
        ```
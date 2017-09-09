---
title: webpack
date: 2017-03-14 09:55:25
tags: [webpack,module]
categories: node
---
# accumulation

## config

1. devtool
    * eval
    * hidden
    * inline

    * ...source-map
    * cheap-...-source-map
    * cheap-modle-...-source-map
2. performance  
    块过大提示,参考:
    ```js
    performance: {
        maxEntrypointSize: 300000,
        hints: isProd ? 'warning' : false
    },
    ```

## plugins

1. html-webpack-plugin  
    主要用于按照模板替换相关引用部分,如webpack的output部分filename配置为`[name].[chunkhash].js`,而hash每次显然不一样,为了能正常获取而使用该插件,在模板中的link/script稍作配置即可  
    通常给模板即可,其中使用blueimp模板引擎:
    ```js
    const HtmlWebpackPlugin=require('html-webpack-plugin');
    ...
    new HtmlWebpackPlugin({
        template: 'src/index.template.html'
    }),
    ...
    ```
    ```html
    <% for (var chunk of webpack.chunks) {
        for (var file of chunk.files) {
            if (file.match(/\.(js|css)$/)) { %>
    <link rel="<%= chunk.initial?'preload':'prefetch' %>" href="<%= htmlWebpackPlugin.files.publicPath + file %>" as="<%= file.match(/\.css$/)?'style':'script' %>">
    <% }}} %>
    ```
2. CommonsChunkPlugin  
    通常用来整合所使用的库
3. sw-precache-webpack-plugin  
    用于生成service worker file,常用配置:
    ```js
    const SWPrecachePlugin=require('sw-precache-webpack-plugin');
    ...
    new SWPrecachePlugin({
        cacheId:'项目名称',
        filename:'生成的用于sw的文件名',
        dontCacheBustUrlsMatching:/./   不添加缓存超时的文件,/./即均不添加超时.因chunkhash已作为版本控制方式,因此无需使用超时头
        staticFileGlobsIgnorePatterns:/^正则$/    正则匹配不进行预缓存的文件
    }),
    ```
4. friendly-errors-webpack-plugin  如名,nothing to say.  
5. vue-ssr-webpack-plugin  
    用于生成vue-ssr-bundle.json,配合`vue-server-renderer.createBundleRenderer(vue-ssr-bundle.json)`使用:
    ```js
    const bundle = require('./dist/vue-ssr-bundle.json');
             ... = require('vue-server-renderer').createBundleRenderer(bundle)
    ```

## problem

Something happend to me...when I was young.

### webpack rules & loaders  

So simple changed:  
webpack 2.0+将loaders替换为rules，但loaders依然提供部分支持

### Node模块中 webpack打包后__dirname==/

对于node环境，webpack有些特殊配置需要指明，且node本身的一些模块也不需要webpack一起给打包上  
以及为了正常使用__dirname/__filename：

```js
const nodeModules = {};

//过滤node模块
fs.readdirSync('node_modules')
    .filter(function (x) {
        return ['.bin'].indexOf(x) === -1;
    })
    .forEach(function (mod) {
        nodeModules[mod] = 'commonjs ' + mod;
    });

module.exports={
    output:{
        ...
        libraryTarget:'commonjs'        //需配置此项,否则需在externals的每一项前加上commonjs
    }
    ...
    target: 'node',
    node:{
        __dirname: true,
        __filename: true
    },
    //old   externals: nodeModules,
    externals: Object.keys(require('../package.json').dependencies)
    ...
}
```

### UglifyJs ES6

对node模块的压缩而言，webpack@2.3.2的uglifyjs插件还是es5的，需在项目下安装es6版本uglifyjs [Harmony](https://github.com/mishoo/UglifyJS2/commits/harmony)  
浏览器，反正都要用babel转义成es5也就没啥影响了  

### html-webpack-plugin <% %>模板未替换

根据其github的[template-option](https://github.com/jantimon/html-webpack-plugin/blob/master/docs/template-option.md)给出的问题列表,正好包含之前使用的html-loader导致冲突所以替换失败
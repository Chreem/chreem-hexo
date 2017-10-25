---
title: Visual Studio Code 插件及使用集锦
date: 2017-03-21 15:15:18
categories: vscode
tags: extension
---
# Introduction

vscode指北，只为自己看得懂

## git

把命令行换成点鼠标了，还需要多说啥？  
需要提前安装git  
默认情况下会自动添加目录下所有，忽略操作移步git .gitignore(goops)  
SSH等配置，git怎么来这玩意就怎么来  
如需emoji可以参照github的gemoji，相关表情代码：<https://www.webpagefx.com/tools/emoji-cheat-sheet/>

<!-- more -->

## 插件推荐

首先感谢各插件作者，请务必收下我的膝盖👬  

|插件名|感想|
|-|-|
|[background](https://github.com/shalldie/vscode-background) | 如名，提供最多三张背景图分别给三开的代码框<br>第一次需要以管理员权限运行code<br>**因使用了vscode不推荐的方式修改背景，修改后vs会多出[不受支持]** ~~爱用不用😜~~
|Emoji & Emoji Code|分别是VSCode能插入emoji和插入emoji的Unicode代码|
|ESLint|语法提示，对Vue需配合vetur使用|
|IntelliJ IDEA keybindings|快捷键改为JetBrains风格，对于同时用webstorm的我，没毛病……  |
|markdownlint|Markdown语法提示，并不是格式化，提示哪里有问题……然后修改，嗯<br>怎么说呢，对强迫症来说，顺带就清楚了MD的语法……|
|Material Icon Theme|工作区文件显示对应图标，基本上用得上的文件都有对应的图标|
|REST Client|如名，没啥可说的|
|Settings Sync|同步code的各项配置，需配合github gist token使用，相当必要无需解释|
|~~Vue2 Snippets~~|已换为vetur|
|vetur|Vue代码高亮及格式化，配合ESLint|

### 插件使用问题

* REST Client  
    新建*.http，然后根据文档写就行，对应请求上会出现`Send Request`，可点击也可按ctrl + alt + r  
    若长时间无响应想取消，则ctrl + alt + q
    *相关配置：git中忽略 .http文件*

* markdownlint  
    1. 默认情况下双空格换行会因[MD009](https://github.com/DavidAnson/markdownlint/blob/v0.4.1/doc/Rules.md#md009---trailing-spaces)而提示语法问题，参照作者的文档添加空格数量例外就OK了
    2. 表格内换行使用HTML元素  
        这点实在没啥办法，MD033不推荐MD中使用html元素br换行，然而表格中想换行只能用这玩意所以添加例外
    3. 有序列表序号，默认为`one`, 可按情况改成`ordered`, 以下写法均正确  
        ```md
        <!-- one -->
        1.*****
        1.*****
        1.*****

        <!-- ordered -->
        1.ooooo
        2.ooooo
        3.ooooo
        ```

    最终settings的MD部分配置：
    ```js
    // Markdown语法纠错
    "markdownlint.config": {
        "MD009": {
            "br_spaces": 2
        },
        "MD013": false,             //default example
        "MD029": {
            "style": "ordered"
        },
        "MD033": {
            "allowed_elements": [
                "br"
            ]
        }
    },
    ```

## 常用配置

为何先放插件再放常用配置……毕竟像Vue酱紫需要插件配合（~~神特喵官网推荐插件~~）的得先装插件某些配置才能生效

### Vue相关

只需几步：

```md
1. 安装插件vetur, 最好也装上ESLint
2. 配置*.vue解析方式为vue, 不装插件只能退而求其次解析为html:  
    "files.associations": { "*.vue": "vue" },

3. 在vue中使用emmet规则：  
    "emmet.syntaxProfiles": { "vue-html": "html" },

此时已经能正常显示高亮和格式化代码了，附带代码提示的话：
4. 全局安装ESLint及其插件，没html插件的话，编辑html会有提示但报错：  
    npm i -g eslint eslint-plugin-html
    ...
    eslint --init

5. 配置使用插件解析html
    "eslint.validate": [ "javascript", "javascriptreact", "html" ],
    "eslint.options": { "plugins": [ "html" ] }

6. 跳过此后所有，关机睡觉
7. Ctrl + Shift + P
8. sync upload
```
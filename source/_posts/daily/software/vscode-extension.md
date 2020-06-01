---
title: Visual Studio Code 插件及使用集锦
date: 2017-03-21 15:15:18
categories: daily
tags: ['usage', 'vscode']
---
# 软件配置

## git

需要提前安装git  
默认情况下会自动添加目录下所有，忽略操作移步git .gitignore(goops)  
SSH等配置，git怎么来这玩意就怎么来  
如需emoji可以参照github的gemoji，相关表情代码：<https://www.webpagefx.com/tools/emoji-cheat-sheet/>

<!-- more -->

# 插件整理

* [background](https://github.com/shalldie/vscode-background)
* ESLint
* IntelliJ IDEA keybindings, 快捷键风格修改
* markdownlint, markdown语法纠错
* Material Icon Theme, 工作区文件图标
* REST Client
* Vetur, vue语法提示
* Settings Sync, vscode多端配置同步

## 插件使用问题

### REST Client

    新建*.http，然后根据文档写就行，对应请求上会出现`Send Request`，可点击也可按ctrl + alt + r  
    若长时间无响应想取消，则ctrl + alt + q
    *相关配置：git中忽略 .http文件*

### markdownlint

1. 默认情况下双空格换行会因[MD009](https://github.com/DavidAnson/markdownlint/blob/v0.4.1/doc/Rules.md#md009---trailing-spaces)而提示语法问题，参照作者的文档添加空格数量例外即可

2. 表格内换行使用HTML元素

    MD033不推荐MD中使用html元素br换行，但表格中想换行又只能使用该标签，遂允许之

3. 有序列表序号，默认为`one`, 可按情况改成`ordered`, 以下写法均正确

```md
<!-- one -->
1. *****
1. *****
1. *****

<!-- ordered -->
1. ooooo
2. ooooo
3. ooooo
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

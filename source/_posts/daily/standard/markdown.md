---
title: markdown
date: 2017-03-10 23:38:18
categories: daily
tags: ['usage', 'markdown']
---
# Markdown syntax

常用的MD语法汇总及当初犯下的问题总结，基础语法没啥可说的，各种符号怎么用@baidu

## what happend before

不规范的就不多说了，详情继续往下有链接

* MD009/no-trailing-spaces: Trailing spaces [Expected: 2; Actual: 1]
    结尾不要手痒按空格……

    ```md
    # hehe _______(空格)
    ```

* MD012/no-multiple-blanks: Multiple consecutive blank lines [Expected: 1; Actual: 2]
    行空多了呗
* MD025/single-h1: Multiple top level headers in the same document  
    当初没在意，还想着为啥Github上一级二级标题下面都会接着下划线呀……之类的  
    但hexo的导航会根据一级标题做1、2、3的标记，遂关闭h1检查
* MD026/no-trailing-punctuation: Trailing punctuation in header [Punctuation: '?']
    标题不要跟标点
* MD032/blanks-around-lists: Lists should be surrounded by blank lines  
    32同时会引出一个语法错误，列表内容紧接着的一行应该要但可以不用空四格。遇到没空出来，多行数据就GG，错位多次都是泪😢

<!-- more -->

## rules

Visit MD001-MD044 at <https://github.com/DavidAnson/markdownlint/blob/v0.4.1/doc/Rules.md>  
Visual Studio Code extension:markdownlint, 相关部分移步VS Code

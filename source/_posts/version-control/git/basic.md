---
title: Git basic usage
date: 2017-03-09 21:48:27
tags:
---
# git

## 建立远程仓库并自动同步提交至web项目

<https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000>

```md
1. 本地直接通过SSH建立远程连接
2. 远程新建git的bare仓库项目  
    该仓库只保存提交历史不允许工作区操作
3. 远程web同步:
    ```bash
    # 进入远程裸库的hooks
    root@remote:/git-server-path# cd hooks

    # 新建文件用于同步工作区, 文件名随意
    root@remote:/git-server-path# vi post-receive

    # 执行命令
    #!/bin/bash
    git --work-tree=/web-project-path  checkout -f
```

<!-- more -->

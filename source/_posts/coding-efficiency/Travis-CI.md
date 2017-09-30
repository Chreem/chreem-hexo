---
title: Travis CI + Coveralls
date: 2017-03-21 15:15:40
tags: [test,travis-ci]
categories: 环境
---
# Travis-CI + Coveralls 自动测试发布

忘了在哪看的一句,

> 好的项目构建测试发布一键完成, 烂家伙全手动  
>—— ~~chreem~~

原话记不大清了, 总之作为码农, 放着能简化的流程不去搞自己全手动什么的是绝壁不能忍的.  
正好这又看到了各项目下都会多出的一个build passing的badge, 顺手百度, 顺手就用上了 ~~然后就停不下来了~~  
此处以之前写的一个node命令行下的rest mock-server为栗🌰, 看看这一溜的做法.  
粗略总结:

```md
1. 跳过第二步
2. 码代码
3. 在第二步顺便就生成了package.json
4. 网上抄一个.travis.yml
5. 写个1==1的测试用例
6. 可有可无的覆盖率测试
```

<!-- more -->

## 码代码

过程中能同时写测试最好, 首先需要写出的代码能测试的了...  
跑测试用的库: `mocha`&`chai`

## 顺便生成的package.json

这个通过`npm init`和依赖安装过程`npm i -S xxx`&`npm i -D xxx`基本上不用担心了, 不过要发布至npm还有些需要微调的部分:

1. bin  用于定义全局安装时命令执行入口
2. preferGlobal 配合1.使用, 告诉npm这个包最好全局安装  
3. script   这其中主要是test需要能够正确执行以便travis运行
4. bugs     也是给npm用的,如名
5. keywords 可有可无

需要注意的是version部分每次合并前都要增加,否则npm publish失败.  
Coveralls覆盖率测试需要用到的两个依赖: `coveralls`&`istanbul`

附图一张:  
![npm](https://cloud.githubusercontent.com/assets/12951147/24762213/4c59593c-1b20-11e7-9b29-a860a3b298b5.png)

## 各站点配置

### Github

最不需要配置的部分,之前怎么用这里还怎么用  
但是!因为Travis-CI是根据主分支变化来执行publish动作的, 再者说直接在master分支上做改动是个很不好的习惯.

### Travis-CI

因为npm的发布需要token, 这个可以在npm login后得到一个(每次登录都会有一个...), 通常会保存在`~/.npmrc`中, 而需要在Travis-CI里配置的也主要是这个token. ~~毕竟没人愿意让自己的高权限token能随便就被看到吧...~~  
所以在项目下配置环境变量:  
![travis-ci](https://cloud.githubusercontent.com/assets/12951147/24762516/3837ac82-1b21-11e7-99e8-010723fb5045.png)

### Coveralls

创建账号, 大功告成...

## 抄一份.travis.yml

```yml
language: node_js           # nothing to say
node_js:
  - 7.7                     # 支持版本, 根据实际情况写

after_script:
  - npm run cover           # 配合Coveralls, 跑覆盖率测试

deploy:
  provider: npm             # 部署位置
  email: $npm_login_email
  api_key: $npm_token_key
  on:
    branch: master          # 发布的分支
```

## 编写测试用例

随便写写就OK了...

## SSH免密

官方的github pages教程用的token, 还没尝试过(或者说没试成功)  
SSH Key参照<http://blog.csdn.net/u012373815/article/details/53574002>  
步骤:  

```md
1. 前置ruby-dev
2. gem install travis
3. 加密私钥及预配ssh_config
4. 修改.travis.yml
5. push测试
```

### 配置私钥

```bash
> touch/vim ssh_config

Host github.com
User git
StrictHostKeyChecking no
IdentityFile ~/.ssh/id_rsa
IdentitiesOnly yes

> travis login --auto
> travis encrypt-file id_rsa --add
# 此步之后会自动在.travis.yml上加上相应解密命令且自动生成对应环境变量
# 但生成之后所在目录及权限均还需修改

> vim .travis.yml

before_install:
- openssl aes-256-cbc -K $encrypted_xxxxxxxxx_key -iv $encrypted_xxxxxxxxx_iv
  -in id_rsa.enc -out ~/.ssh/id_rsa -d
- chmod 600 ~/.ssh/id_rsa
- eval $(ssh-agent)
- ssh-add ~/.ssh/id_rsa
- cp ssh_config ~/.ssh/config
- git config --global user.name "xxx"
- git config --global user.email "xxx"
```

---
title: How this work
date: 2017-03-03 9:14:23
tags: [nginx,git]
---
# Before

[![完成度](https://img.shields.io/badge/Chreem--Hexo-95%25-yellowgreen.svg?style=flat-square)](http://hexo.chreem.xyz)  
About a month ago, young ignorance Chreem bought a QCloud VM that cost 60+￥/month !!! with just 1M bandwith !!!.
So I'm ready to migrate my site to ConoHa.Not only cheaper than QCloud average 10￥, but also 100M :P.Just need to pay attention to the exchange rate.
<!-- more -->

## How this work

### Site

#### V1.1

1. domain binding changed
    Finally, I figured out, and bound [hexo.chreem.xyz](http://hexo.chreem.xyz) on ConoHa.  
    ~~Forget about QCloud.~~  
2. https
    And enabled my site's https, just like what I have done to <http://chreem.xyz> on QCloud.  
    Of course not my own CA cert, otherwise it would not be a lot of people stopped by the browser...

### ~~V1.0~~

Before my QCloud expired, the domain of mine chreem.xyz would not binding on ConoHa, I use the Nginx on my tencent vm pretending to be redirected to reverse proxy to Japan.& my building blog will need to work at the same time WITH HTTPS...So that I have to write a strange configuation.
Hexo site is deployed on my ConoHa vps.~~(Too poor to use multiple marchines, container instead)~~

```md
The entire site looks like this:
(omit the response&HTTPS SSL etc.)
                            +———————————QCloud——————————+       +———————————ConoHa——————————+
  http://hexo.chreem.xyz    |                           |       |                           |
--------------------------->|-->:80{ upstream ConoHa }  |------>|-->:80{ (same as QCloud    |
                            |                           |       |         443 upstream)     |
    http://chreem.xyz       |                           |       |      }                    |
--------------------------->|-->:80{ rewrite to :443}   |       |                           |
    https://chreem.xyz      |                           |       +———————————————————————————+
--------------------------->|-->:443{ upstream docker}  |
                            |          |                |
                            |          |  +-container1-+|
                            |          ├->|-->:8000    ||
                            |          |  +————————————+|
                            |          |                |
                            |          |  +-container2-+|
                            |          ├->|-->:8000    ||
                            |          |  +————————————+|
                            |          |                |
                            |          |  +-container3-+|
                            |          └->|-->:8000    ||
                            |             +————————————+|
                            +———————————————————————————+
```

### Performance optimization

About this problem, for example all nginx make gzip on (upstream is a proxy after all), & just QCloud's nginx need to turn the cache-control on. Especially cache-control, it's necessary for a website on foreign host.
Visit more from a **BIG LEG's** [博客优化之路](http://pandasjw.com/2017/02/27/website-quick/).

## Github

Except my own site, I deploy on github also, and have two branch for site&post.

### [branch master](https://github.com/Chreem/chreem.github.io)

hexo deploy branch, used ot show on the [home page](https://chreem.github.io).
Install deployer in hexo init directory at first if you use high version hexo.

```bash
$ npm install --save hexo-deployer-git
$ vi _config.yml
Shift + G
> ...
    deploy:
    type: git
    repo: git@github.com:Chreem/chreem.github.io.git
    branch: master
```

### [branch blogs](https://github.com/Chreem/chreem.github.io/tree/blogs)  

posts, all my blogs.  
ConoHa:Make sure your rsa key in `~/.ssh/`

```bash
$ cd blog/source/_posts
~/blog/source/_posts$ git init
~/blog/source/_posts$ git remote add origin git@github.com:Chreem/chreem.github.io.git
~/blog/source/_posts$ git add *
~/blog/source/_posts$ git commit -m "hexo blog init"
~/blog/source/_posts$ git push origin master:blogs
```

my compute(windows): so as linux, in `C:\Users\yourname\.ssh\`. And installed [Visual Studio Code](https://code.visualstudio.com/)

```cmd
~\> code
Ctrl + `
PS ~\> git init
PS ~\> git remote add origin git@github.com:Chreem/chreem.github.io.git
PS ~\> git pull origin blogs:master
```

## Timing Update

It's so simple by using command of Linux: `crontab`  
For example, I want my site update from github in every night 0:00 automatically.Follow these command:  

1. First need a timing task:
    ```bash
    $ vi timing.cron
    < 0 0 * * * (update commands)
    $ crontab timing.cron
    ```
    So that timing task is auto-running, you can check this tack by `crontab -l`.  
    Visit more about [crontab](https://www.baidu.com/s?wd=crontab) command.

2. This task need to run a shell to ensure we can change update commands rather than `crontab -r` & `crontab timing.cron` every times  
    **The following complete directory is omitted:**
    ```bash
    $ vi update.sh
    < #!/bin/bash
    < cd .../_ports/
    < git pull -f origin blogs:master
    < cd .../hexo-blog/
    < hexo d -g

    vi timing.cron
    < 0 0 * * * exec ..../update.sh

    # Don't for get the permission of update.sh
    $ chmod 755 update.sh

    $ crontab timing.cron
    ```

In short, I can not update too frequently, energy is used to write articles rather than update site.  
~~I'm lazy anyway~~.

## Trouble

### `# Service nginx start`

When I run this command & failed:  
`> Job for nginx.service failed because the control process exited with error code. See "systemctl status nginx.service" and "journalctl -xe" for details.`  
I don't remember where I copied this nginx config. Although it has a user named nginx, not mentioned at all...Before I deleted this line, I modified worker_processes, run `systemctl status nginx.service` as Shell told, delete reverse server etc..Anyway I had thought.Finally I delete all nginx.conf in a fit of anger, started it over without user line.

```nginx
user nginx;
worker_processes auto;
events{
    ...
}
...
```

In fact, high version nginx first line is configed `user www-data`, and I don't have any reason **temporary** to change it.

### Template render error: (unknown path)

At first I toss about my hexo config for a long time, so was github.
Finally I found in <https://github.com/hexojs/hexo/issues/2384>
Because of my post appeared nunjucks double brace syntax. Fix it, that will do.
~~So how could I type {% raw %}{{}}{% endraw %}?!!!~~

### The display page shows the whole article

Just need a `<!-- more -->` label like this:

```md
> 文章简介
> <!-- more -->
> 正文
```

### `git pull` in cron bash update.sh autorun failed

Actually, the local file has been modified

```bash
root@150-95-139-228:~/site/chreem-hexo/source/_posts# git pull origin blogs
From github.com:Chreem/chreem.github.io
 * branch            blogs      -> FETCH_HEAD
Already up-to-date.
```

But I'm pretty sure I don't need this version, just use what I pushed from my win$s, and I can checkout all change first, than make update.sh like this:

```bash
#!/bin/bash
cd ~/site/chreem-hexo/source/_posts/
git checkout -- *
git pull origin blogs:master
cd ~/site/chreem-hexo/
hexo d -g
date >> ~/site/TimingUpdate/count.log
```

## Used&Thank for

Blog:       [hexo](https://hexo.io)  
Blog-Theme:[cyanstyle](https://github.com/wizardforcel/hexo-theme-cyanstyle) [Viosey's material](https://github.com/viosey/hexo-theme-material) [spfk](https://github.com/luuman/hexo-theme-spfk)  
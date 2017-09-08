---
title: Git Remote
date: 2017-03-05
tags: [git,ssh]
categories: tools
---
# Git remote

Git is currently very widely used. This article outlined git remote repertory. Of course you can check more detail on [~~Ruanyifeng laoshi~~ Teacher ruan's post](http://www.ruanyifeng.com/blog/2014/06/git_remote.html).  
[So how to install git?](https://www.baidu.com) (~~`apt-get install`~~)

## git clone

Above all, you should get someone's code then we can talk others. Looks like a simple command? Without any protect, it's certainly simple. For example, I want to clone my project from github to folder ~/website/project1:

```bash
cd ~
git clone https://chreem.github.com/blog website/project1
```

Wait a while, it's done. What if someone is so lazy that don't want to type password, and use SSH?(Sure, you can use SSH key rather than password, it's safer than a symmetric cryptography that could be ~~exploded~~ brute force attack to uses the asymmetric one.)  Some cloud service provider like RedHat OpenShift also use SSH as git links. You would use command like always:

```bash
git clone ssh://randomuser@project-yourname.rhcloud.com/~/git/project.git/
```

<!-- more -->

### Get rsa key-pair

#### create

```bash
# Makesure your ~/.ssh/ is empty or without key-pair is named id_rsa&id_rsa.pub

$ ssh-kengen -t rsa -C "your-email@domain.xxx"
# There has the key fingerprint. You could press double enter if you are lazy enough like me.

$ cat ~/.ssh/id_rsa.pub
> .....
# Then you can paste the displayed content to where need the SSH key like step 2. that not only git link site.
# I'm using the same key-pair to login my vm & clone git project.
```

#### assign for different site

Every site with same key-pair seems without problem, seems...  
So you also can assign different key-pairs for different sites or projects:  

```bash
cd ~/.ssh/
vi config
< Host github.com
<      PreferredAuthentications publickey
<      IdentityFile ~/.ssh/id_rsa
<      User 549078191@qq.com
```

### Tell some sites public key

![image](https://cloud.githubusercontent.com/assets/12951147/23585408/853bba0e-01b9-11e7-82cb-4b3689b03f03.png)  
![image](https://cloud.githubusercontent.com/assets/12951147/23585421/1c19a1fc-01ba-11e7-95e7-a9abd917b259.png)

### Clone someone's project

```bash
$ git clone ssh://chreem@rhcloud.com/test.git
# > RSA key fingerprint is ***.
# Are you sure you want to continue connecting (yes/no)? yes
```

## git remote & git pull

This command is used to get your own or team project. `git clone` as its name copied project as yours.

### Add remote ~~host~~ repertory and pull it

```bash
$ git remote add origin git@github.com:chreem/test.git
$ git pull origin remote-branch:local

# always master for me, so as remote and local...so the command could be:
$ git pull origin master

# without multi branches and only one remote
$ git pull
```

### notice

If you changed some files after first pull, other team member push a new version too, and his code is better than yours, how would you do? Of course ~~let him treat you to dinner~~, pull again.
So if you would save his very sure, can add `-f` param:

```bash
git pull -f origin master
```

## git push

Every monkey has its day. It's turn that ~~you treat team members~~ push your code.

```bash
git push origin local-branch:remote
# It's similar to git pull just notice the branches: where the code is--->where you want to put
```

Finally copy a picture from <http://www.ruanyifeng.com/blog/2014/06/git_remote.html>  
![Git](http://image.beekka.com/blog/2014/bg2014061202.jpg)
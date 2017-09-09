---
title: 日常使用
date: 2017-09-09 21:48:27
tags:
---
# wall姿势

收集各种姿势

## 增强

* bbr加速  
    开启姿势:
    1. linux内核版本≥4.9
        ```bash
        # uname -r                      # 查看内核版本
        4.4.0-57-generic
        # apt-cache search linux-image  # 查看可下载的内核
        # apt-get install linux-images-4.10.0-21-generic
        # apt-get remove linux-images-4.4.0-57-generic
        # reboot
        ```
    2. 启用bbr
        ```bash
        > vi /etc/sysctl.conf   #在最后加上:
        net.core.default_qdisc=fq
        net.ipv4.tcp_congestion_control=bbr
        # 保存退出
        > sysctl -p
        ```
    3. `lsmod | grep bbr` 查看是否出现类似输出
        ```bash
        tcp_bbr                20480  3
        ```

## 各种全家桶

### shadowsocks

需要:

* [shadowsocks win客户端](https://github.com/shadowsocks/shadowsocks-windows)

服务端安装使用(ubuntu 16.04):

1. apt-get install shadowsocks
2. touch ss/ssserver.json
3. 编辑配置文件:
    ```json
    {
        "server" : "vps的ip",
        "server_port" : 想使用的端口,
        "local_address" : "127.0.0.1",
        "local_port" : 1080,
        "password" : "如名",
        "timeout" : 300,
        "method" : "加密方式",
        "fast_open" : false
    }
    ```
4. 后台运行: `nohup ssserver -c ss/ssserver.json start &`
5. [停止]: `netstat -apn` + `kill ...`

### SQSX

前段时间还一直用

需要:

* 满世界找资源
* 满世界尝试可用服务器

### GoAgent

过去式+1,同样为刚入学

需要:  

* Google账号 - 单列一项,嗯...没途径出门哪来账号
* Google GoAgent - proxy program
* Proxy SwitchyOmega - Chrome扩展

### FreeGate

过去式,刚入坑大学时用的玩意

需要:

* 满世界找资源(下载链接不固定)
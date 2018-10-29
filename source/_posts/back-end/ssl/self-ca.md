---
title: 自行签发CA证书
date: 2018-06-29 22:12:09
categories: ssl
tags: https
---
# 自签发CA证书 for HTTPS

## Create Root CA

1. 写好配置文件conf.cnf，预置SAN信息

    ```md
    # ...

    [ SAN ]
    subjectAltName	= DNS:*.chreem.club,DNS:chreem.club
    ```

2. 生成rsa密钥

    ```bash
    > openssl genrsa -des3 -out server.enc.key 2048   # 生成rsa
    > password
    > openssl rsa -in server.enc.key -out server.key    # 写入密码
    ```

3. 生成证书

    ```bash
    > openssl req -new -sha256  \
        -x509 \
        -days 36500 \
        -key server.key \
        -subj "/C=CN/ST=Hubei/L=Wuhan/O=ChreemTech/OU=FS/CN=chreem.club" \
        -extensions SAN \
        -config <(cat ./conf.cnf) \
        -out server.crt
    ```
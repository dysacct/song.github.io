---
title: SSL证书验证
date: 2025-07-31 10:09:56
categories:
  - harbor
---

```bash
apt install -y openssl

openssl x509 -noout -modulus -in nginx.baby.pem | openssl md5
-------------------------------- 你的公钥证书(certificate)------
openssl rsa -noout -modulus -in nginx.baby.key | openssl md5
-------------------------------- 你的私钥(private key)----------
```
MD5(stdin)= e3b0c44298fc1c149afbf4c8996fb924
MD5(stdin)= e3b0c44298fc1c149afbf4c8996fb924   <-- 一样，说明匹配

若不一致，说明你的 key 与 pem 不匹配，Harbor 启动时会报错 SSL routines:SSL_CTX_use_PrivateKey_file: key values mismatch。

> 自签证书

```bash
mkdir -p /data/cert
openssl req -newkey rsa:4096 -nodes -sha256 -keyout /data/cert/harbor.key -x509 -days 365 -out /data/cert/harbor.crt
```

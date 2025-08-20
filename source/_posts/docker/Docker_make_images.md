---
title: 制作docker镜像
date: 2025-07-30 17:26:29
tags:
  - docker镜像
categories:
  - Docker
---

##  docker 镜像

### Dockerfile
```dockerfile
FROM nginx:latest

COPY nginx.conf /etc/nginx/nginx.conf
COPY htpasswd /etc/nginx/.htpasswd
COPY html/ /usr/share/nginx/html/
COPY ssl/ /etc/nginx/ssl/
```

### 制作镜像
```bash
docker build -t mynginx:ssl .
# mynginx:ssl 为镜像名称 . 为当前目录
docker build -t test_nginx_image_backup .
# test_nginx_image_backup 为镜像名称
# . 为当前目录
```

### 运行镜像

```bash
docker run -itd \
  --name mynginx_ssl \
  --restart unless-stopped \
  -p 80:80 -p 443:443 \
  mynginx:ssl


docker run -itd --name test_nginx --restart unless-stopped test_nginx_image_backup
```

### 打成 tar 包

```bash
docker save -o mynginx.tar mynginx:ssl
docker save -o test_nginx_image_backup.tar test_nginx_image_backup
```

### 在其他服务器导入镜像

```bash
docker load -i mynginx.tar

docker load -i test_nginx_image_backup.tar
```


## 远程harbor管理docker

### 
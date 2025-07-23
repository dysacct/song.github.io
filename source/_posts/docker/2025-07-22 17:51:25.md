---
title: Docker 安装 MySQL
date: 2025-07-22 17:51:25
tags:
  - MySQL
categories:
  - Docker
---

## 一、用 Docker 安装 MySQL 8.0 的基本命令

```bash
docker run -d \
  --name mysql8 \
  -e MYSQL_ROOT_PASSWORD=song@123 \
  -p 3306:3306 \
  mysql:8.0
```

说明：

- -d：后台运行容器。

- --name mysql8：容器名为 mysql8。

- -e MYSQL_ROOT_PASSWORD=song@123：设置 root 用户密码。

- -p 3306:3306：将宿主机 3306 端口映射到容器内的 3306 端口。

- mysql:8.0：使用官方 MySQL 8.0 镜像。

### -v 参数解释

`-v` 是 挂载卷（volume） 的意思，让容器里的文件和你本地的文件夹互通。你可以：

```bash
-v <主机路径>:<容器路径>
```

```bash
-v /my/mysql/data:/var/lib/mysql
```

就会把宿主机 /my/mysql/data 文件夹挂到容器里的 /var/lib/mysql，也就是 MySQL 的数据目录。

#### 用 `-v` 方式导入初始化数据

MySQL 支持在容器启动时，自动执行挂载目录中 .sql 文件来初始化数据库。

1. 准备 SQL 文件:

```sql
-- 文件名：init.sql
CREATE DATABASE testdb;
USE testdb;
CREATE TABLE user (id INT PRIMARY KEY, name VARCHAR(20));
INSERT INTO user VALUES (1, 'Alice'), (2, 'Bob');

```

```bash
docker run -d \
  --name mysql8-init \
  -e MYSQL_ROOT_PASSWORD=song@123 \
  -p 3306:3306 \
  -v /home/user/mysql-init:/docker-entrypoint-initdb.d \
  mysql:8.0
```

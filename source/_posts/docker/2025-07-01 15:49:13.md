---
title: Docker 用法
date: 2025-07-01 15:49:13
tags:
  - Docker 用法
categories:
  - Docker
---

# Docker 基础用法

### 拉取镜像

**配置加速器**

```bash
对于使用 upstart 的系统而言，编辑 /etc/default/docker 文件，在其中的 DOCKER_OPTS 中配置加速器地址：
DOCKER_OPTS="--registry-mirror=https://registry.docker-cn.com"

service docker restart

Ubuntu16.04+、Debian8+、CentOS7
对于使用 systemd 的系统，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）：
{"registry-mirrors":["https://reg-mirror.qiniu.com/"]}

systemctl daemon-reload
systemctl restart docker
```

### 基础命令

```bash
# 查看本地有效镜像
docker images
docker image list
# 下载镜像不同版本（不跟版本号就是默认最新版本）
docker pull ubuntu:11
docker pull ubuntu:12.04
docker pull centos:7
# 运行
docker run  -it  --name 容器名 imagename  /bin/bash
docker run -d 后台
docker search centos * 这种方法只能用于官方镜像库 搜索基于 centos 操作系统的镜像
# 拉取镜像
docker pull centos  注：没有加registry，默认是从docker.io/dockerhub下载的
docker pull daocloud.io/library/tomcat:7
docker pull daocloud.io/library/centos:6
# 查看镜像详细
docker inspect 镜像id/镜像名称
# 删除镜像
docker rmi daocloud.io/library/mysql
docker rmi 81debc
# 删除多个，用空格隔开
或者 docker image rm 镜像名或者镜像id
# 之查看所有镜像id
docker images -q
# 删除所有镜像
docker rmi $(docker images -q)
docker rmi `docker images -q`
docker    create    --name  容器名        镜像名            #创建新容器但不启动
docker    start    容器名/容器ID                       #运行容器
docker    restart    容器名/容器ID                     #重启容器
docker    stop    容器名/容器ID                       #停止容器
docker    kill    容器名/容器ID                       #强制停止容器
docker     stop/kill    $(docker ps -q)             #停止所有运行中的容器
docker    pause    容器名/容器ID                       #暂停容器
docker    nopause    容器名/容器ID                     #恢复容器的运行
docker    rename    旧容器名    新容器名               #修改容器名
docker    stats    容器名/容器ID                       #实时显示容器资源使用统计信息
docker    inspect    容器id/容器名                     #查看本地容器详细信息
docker    top        容器id/容器名                       #查看容器内进程详细信息
docker    cp    -a    容器名：路径    本地存放位置          #从容器拷贝到本机
docker    cp    -a    本地文件路径    容器名：路径          #从本机拷贝到容器
docker image save <image_name>:<tag> >/to/path/<image_name>.tar.gz   #导出镜像
docker image load -i /to/path/<image_name>.tar.gz   #导入镜像
docker export my_container > container.tar expo    # 导出容器
docker import  my_v3.tar nginx/nginx:v4   # 导入容器
```

```bash
docker run -d \
  --name mysql8 \
  -e MYSQL_ROOT_PASSWORD=song@123 \
  -p 3306:3306 \
  mysql:8.0
```

- -d：后台运行容器。

- --name mysql8：容器名为 mysql8。

- -e MYSQL_ROOT_PASSWORD=song@123：设置 root 用户密码。

- -p 3306:3306：将宿主机 3306 端口映射到容器内的 3306 端口。
- -v <主机路径>:<容器路径>

```bash
docker exec -i mysql8 mysql -uroot -pYOUR_PASSWORD < /backup.sql # 直接导入之歌数据库
```

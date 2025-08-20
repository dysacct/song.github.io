---
title: Docker 进阶用法
date: 2025-07-31 14:00:57
categories:
  - Docker
---

### 查询容器

```bash
docker ps -aqf "ancestor=mysql:8.0" # 找出镜像是 mysql:8.0 的容器
docker ps -aqf "status=exited" # 找出已退出的容器
docker ps -aqf "name=^mysql8" # 找出名字以 mysql8 开头的容器
```

- exited：容器已退出

- running：容器正在运行

- created：容器已创建但未运行

- paused：容器被暂停

- restarting：容器正在重启

- dead：容器进程已经终止但还未清理

### 查询容器重启策略

```
docker run -itd --name test_nginx --restart unless-stopped test_nginx_image_backup
```

```bash
docker inspect -f '{{.HostConfig.RestartPolicy.Name}}' <容器名\id>
```

```bash
docker update --restart=always <容器名\id>    # 修改已有容器的重启策略
```

|      策略      |                     说明                     | 系统重启后 | 异常退出 | 正常退出 | 手动停止后 |
| :------------: | :------------------------------------------: | :--------: | :------: | :------: | :--------: |
|   no（默认）   |                  不自动重启                  |     否     |    否    |    否    |    N/A     |
|     always     |                 总是自动重启                 |     是     |    是    |    是    |   会重启   |
| on-failure[:N] |          非 0 退出才重启，最多 N 次          |     否     |    是    |    否    |     否     |
| unless-stopped | 总是自动重启，但如果你手动停过它，就不再启动 |     是     |    是    |    是    |  不会重启  |

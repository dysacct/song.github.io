---
title: git ssh不可用时
tags: 
  - git
date:  2025-08-18 14:02:21
categories:
  - git
---

## 概述
git ssh 不可用时用https方式

### 使用
1. 换成 HTTPS 方式（最简单）

如果你不强依赖 SSH，可以直接改用 HTTPS：
```bash
git remote set-url origin https://gitee.com/你的用户名/你的仓库名.git
```
2. 使用 SSH 443 端口

Gitee 提供了基于 443 端口的 SSH 连接方式，绕过 22 端口限制。

修改 ~/.ssh/config 文件（如果没有就新建）：
```ssh
Host gitee.com
    HostName gitee.com
    User git
    Port 443
```

然后在运行
```bash
git push -u origin master
```
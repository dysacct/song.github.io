---
title: find 命令
date: 2025-08-15 17:32:01
categories:
  - Linux
---

## 概述

find 命令可以根据指定的条件，去文件系统中搜寻符合条件的文件

### 使用

```bash
find -type f # 查找普通文件
find -type d # 查找文件夹
find / -perm 644 # 查找权限为644的文件
find -atime +1 -size 50k  # 查找最后修改时间大于1天的文件，同时是50k
find -maxdepth 3 -type d -name "nginx" # 查找名字为nginx的文件夹，最大搜索深度是三级
find -maxdepth 3 -type d -not -name "nginx" # 查找名字不包含nginx的文件夹，最大搜索深度是三级
```

```bash
find /* -name "nginx"

# 查找根下所有的nginx目录参数说明

-name
根据文件名查找文件，-name 后面可以使用通配符
```

```bash
find ~/ -type f -name "1.txt" 2>/dev/null -exec sh -c 'echo "hello" >> {}' \;
# 查找当前目录下的所有.txt文件，并且给这些文件后面添加内容
find ~/ -type f -name "1.txt" 2>/dev/null | xargs -I@ sh -c 'echo "hello" > @'
# 第二个方式，上面方式不支持对文件夹处理
```

---
title: smart 磁盘检查
date: 2025-07-23 14:36:17
tags:
  - 磁盘检查
categories:
  - Linux
---

## smart 介绍

smartclt

## 使用

```bash
lsblk -d -o NAME,SIZE,MODEL
NAME  SIZE MODEL
sda 278.9G PERC H330 Mini
sdb   5.5T PERC H330 Mini
sdc   5.5T PERC H330 Mini
sdd   5.5T PERC H330 Mini
```

```bash
smartctl -a /dev/sda
```

系统使用了 DELL 或 MegaRAID（如 PERC 控制器） 管理磁盘；
/dev/sdb 实际只是一个 RAID 虚拟设备，smartctl 无法直接对它读 S.M.A.R.T 信息；
smartctl 需要你告诉它 RAID 卡后面“具体是哪个物理盘”——通过 -d megaraid,N 参数。

```bash
smartctl -a -d megaraid,1 /dev/sdb | grep -E "^SMART Health Status" | awk -F ":" '{print $NF}' | tr -d " "
```

---
title: nft 使用
date: 2025-05-28 11:05:44
tags:
  - "nft"
categories:
  - "nft"
---

# nft 使用

### 1. 安装 nft 工具

```bash
apt-get install nftables
yum install nftables
```

### 2. 配置 nft 规则

```bash

# 3. 保存 nft 规则
nft list ruleset
```

```bash
meta skuid 1000 counter 的组合表示：

匹配条件：只处理与 UID 为 1000 的用户关联的套接字的数据包。
动作：对匹配的数据包进行统计，记录数据包数量和字节数。
```

---
title: Nginx删除脚本
date: 2025-07-30 11:14:47
tags:
  - Nginx
categories:
  - shell
---

```bash
#!/bin/bash

# 颜色
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[0;33m'
BLUE='\033[0;34m'
PURPLE='\033[0;35m'
CYAN='\033[0;36m'
NC='\033[0m'

echo -e "${GREEN}=== 开始完全卸载 nginx ===${NC}"

# 停止服务
echo -e "${YELLOW}1. 停止 nginx 服务...${NC}"
sudo systemctl stop nginx 2>/dev/null || true # true 表示忽略错误
sudo systemctl disable nginx 2>/dev/null || true

# 卸载软件包
echo -e "${YELLOW}2. 卸载 nginx 软件包...${NC}"
sudo apt remove --purge -y nginx nginx-common nginx-full nginx-core 2>/dev/null || true
sudo apt autoremove --purge -y
sudo apt autoclean

# 删除配置和数据目录
echo -e "${YELLOW}3. 删除配置文件和目录...${NC}"
sudo rm -rf /etc/nginx
sudo rm -rf /var/log/nginx
sudo rm -rf /var/cache/nginx
sudo rm -rf /var/run/nginx.pid
sudo rm -rf /var/lib/nginx

# 删除服务文件
echo -e "${YELLOW}4. 删除服务文件...${NC}"
sudo rm -f /lib/systemd/system/nginx.service
sudo rm -f /etc/systemd/system/nginx.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/nginx.service

# 删除用户和组
echo -e "${YELLOW}5. 删除 nginx 用户和组...${NC}"
sudo deluser nginx 2>/dev/null || true
sudo delgroup nginx 2>/dev/null || true

# 重新加载 systemd
echo -e "${YELLOW}6. 重新加载 systemd...${NC}"
sudo systemctl daemon-reload

# 检查残留
echo -e "${YELLOW}7. 检查残留文件...${NC}"
echo -e "${YELLOW}检查是否还有 nginx 进程:${NC}"
ps aux | grep nginx | grep -v grep || echo -e "${RED}没有 nginx 进程${NC}"

echo -e "${YELLOW}检查是否还有 nginx 包:${NC}"
dpkg -l | grep nginx || echo -e "${RED}没有 nginx 包${NC}"

echo -e "${YELLOW}检查端口 80 使用情况:${NC}"
sudo netstat -tlnp | grep :80 || echo -e "${RED}端口 80 未被占用${NC}"

echo -e "${GREEN}=== nginx 卸载完成 ===${NC}"
echo -e "${YELLOW}建议重启系统以确保完全清理${NC}"
```

---
title: ansible 问题解决
date: 2025-05-29 10:51:47
tags:
  - "ansible"
categories:
  - "ansible"
---

# 强制客户端使用 IPv6
既然不可连接的机器只监听 IPv6，可以尝试强制 scp 使用 IPv6：
确认服务器的 IPv6 地址：
```bash
ip -6 addr show
scp -6 -i ~/.ssh/BDKJ-1025.rsa /apps/data/workspace/ansible/scripts/default.sh root@[IPv6_ADDRESS]:/tmp/a.sh
```

# 主机清单（inventory）中使用 IPv6 地址
```bash
[webservers]
web1 ansible_host=[2001:db8::1] ansible_user=root
web2 ansible_host=[2001:db8::2] ansible_user=root
```
IPv6 连接可能需要你额外指定 ansible_ssh_common_args 来确保 ssh 使用正确的方式连接。
```bash
[webservers]
web1 ansible_host=[2001:db8::1] ansible_user=root ansible_ssh_common_args='-6'
web2 ansible_host=[2001:db8::2] ansible_user=root ansible_ssh_common_args='-6'
```
如果你使用 YAML 格式的 Inventory 文件（主机清单）
```bash
all:
  hosts:
    web1:
      ansible_host: "[2001:db8::1]"
      ansible_user: root
      ansible_ssh_common_args: "-6"
    web2:
      ansible_host: "[2001:db8::2]"
      ansible_user: root
      ansible_ssh_common_args: "-6"
```

# 启用 IPv4 监听
不可连接的机器只监听 IPv6，需修改配置让 sshd 同时监听 IPv4：
```bash
cat /etc/ssh/sshd_config
```
确保没有限制为 IPv6 的配置：
```bash
AddressFamily inet6  # 如果存在，改为 AddressFamily any 或注释掉
ListenAddress ::     # 如果存在，添加 ListenAddress 0.0.0.0
```
推荐配置
```
AddressFamily any
ListenAddress 0.0.0.0
ListenAddress ::
```
# 检查 ssh.socket 配置：
```bash
cat /usr/lib/systemd/system/ssh.socket
```
确保监听 IPv4 和 IPv6：
```bash
[Socket]
ListenStream=0.0.0.0:22
ListenStream=[::]:22
Accept=yes
```
> 如果缺少 IPv4 监听，添加 ListenStream=0.0.0.0:22。

```bash
systemctl daemon-reload
systemctl restart ssh.socket
```

# 修复 SFTP 子系统
```bash
grep Subsystem /etc/ssh/sshd_config
# Subsystem sftp /usr/lib/openssh/sftp-server
ls -l /usr/lib/openssh/sftp-server
# 验证是否存在
apt update && apt install openssh-sftp-server
# 如果不存在
systemctl restart ssh.socket
```
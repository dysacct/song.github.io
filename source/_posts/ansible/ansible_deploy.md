---
title: ansible 配置
date: 2025-05-28 17:29:28
tags:
  - ansible
categories:
  - ansible
---

# ansible 配置

```bash
[defaults]
forks          = 10
ansible_ssh_common_args = -o PreferredAuthentications=publickey,password -o PubkeyAuthentication=yes
remote_port    = 22
inventory      = /apps/data/workspace/ansible/hosts.ini
host_key_checking = False
stdout_callback = minimal
timeout = 10
remote_user = root
log_path = /apps/data/workspace/ansible/ansible.log.log
private_key_file = ~/.ssh/id_rsa

[ssh_connection]
pipelining = True
scp_if_ssh = True
```

| Section          | Option                                                                                             | Value                                                                 | Description                                                                                    |
| ---------------- | -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| [defaults]       | forks                                                                                              | 10                                                                    | 设置并行执行任务的最大进程数，允许同时处理 10 台主机以提高效率                                 |
| [defaults]       | ansible_ssh_common_args -o PreferredAuthentications=publickey,password -o PubkeyAuthentication=yes | 指定 SSH 认证优先顺序为公钥认证，失败后尝试密码认证，并启用公钥认证。 |
| [defaults]       | remote_port                                                                                        | 22                                                                    | 指定默认 SSH 端口为 22，可通过 inventory 文件覆盖。                                            |
| [defaults]       | inventory                                                                                          | /apps/data/workspace/ansible/hosts.ini                                | 指定默认 inventory 文件路径，定义 Ansible 管理的主机列表。                                     |
| [defaults]       | host_key_checking                                                                                  | False                                                                 | 禁用 SSH 主机密钥检查，简化初次连接（降低安全性，建议受控环境使用）。                          |
| [defaults]       | stdout_callback                                                                                    | minimal                                                               | 设置输出格式为简洁模式，仅显示关键信息，适合脚本化或减少日志冗余。                             |
| [defaults]       | timeout                                                                                            | 10                                                                    | 设置 SSH 连接超时时间为 10 秒，适合网络稳定的环境。                                            |
| [defaults]       | remote_user                                                                                        | root                                                                  | 设置默认登录用户为 root，需确保远程主机允许 root 登录。                                        |
| [defaults]       | log_path                                                                                           | /apps/data/workspace/ansible/ansible.log.log                          | 指定日志文件路径，记录 Ansible 运行日志，便于调试。                                            |
| [defaults]       | private_key_file                                                                                   | ~/.ssh/id_rsa                                                         | 指定默认 SSH 私钥路径，确保文件存在且权限正确。                                                |
| [ssh_connection] | pipelining                                                                                         | True                                                                  | 启用 SSH pipelining，通过单一连接发送多条命令，显著提高执行速度。需禁用远程主机的 requiretty。 |
| [ssh_connection] | scp_if_ssh                                                                                         | True                                                                  | 启用 SCP 协议进行文件传输，通常比 SFTP 更快。                                                  |

# ansible 主机清单配置

```bash
[temp1]
11.16.11.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"
11.16.12.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"
11.16.13.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"
11.16.14.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"
11.16.15.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"
11.16.16.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"
11.16.17.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"
11.16.18.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"
11.16.20.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"

[temp2]
11.16.19.3 ansible_ssh_private_key_file=~/.ssh/BDKJ-1025.rsa ansible_ssh_user=root ansible_ssh_user="登录用户"
```
# 主机清单常用连接控制变量（汇总表）
| 变量名	| 作用说明	| 示例 | 
| ----  | ----  | ----  |
| ansible_host	| 要连接的目标主机 IP 或域名	| ansible_host=192.168.1.10 或 ansible_host=[2001:db8::1] |
| ansible_user	| 连接时使用的用户名	| ansible_user=root|
| ansible_port	|SSH 端口（默认 22）	|ansible_port=2222|
| ansible_password	| 连接密码（不推荐明文）| ansible_password=yourpassword|
| ansible_private_key_file	|SSH 私钥路径| ansible_private_key_file=~/.ssh/id_rsa |
| ansible_connection	| 连接方式（默认 ssh）| ansible_connection=ssh、local、docker、winrm|
| ansible_ssh_common_args	|传递给 ssh 的额外参数（推荐用于 IPv6、跳板机等）| 	ansible_ssh_common_args='-6 -o StrictHostKeyChecking=no' |
| ansible_shell_type	| shell 类型（sh / csh / powershell）|	ansible_shell_type=sh|
| ansible_python_interpreter	| 指定远程主机 Python 解释器路径|	ansible_python_interpreter=/usr/bin/python3|
| ansible_become	|是否提权（sudo）|	ansible_become=true|
|ansible_become_user	|sudo 切换的目标用户	|ansible_become_user=root|
| ansible_become_method	| 使用 sudo、su、pbrun 等方法提权	|ansible_become_method=sudo|
| ansible_become_password	|sudo 密码（如需）	|ansible_become_password=xxx（不推荐明文）|
如果配置文件写过后，也可以不用添加

# 使用方法
直接使用 -i 指定主机清单,使用 --config 指定配置文件路径
>注意注意注意!!!
如果你的 ansible 不支持–config 参数,那么你直接设置环境变量: export ANSIBLE_CONFIG=/apps/data/workspace/ansible/ansible.cfg

然后命令里面的–config 去掉,再执行就可以了
```bash

```

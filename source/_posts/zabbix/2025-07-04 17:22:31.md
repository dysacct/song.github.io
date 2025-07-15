---
title: Zabbix被监控端自动被采集
date: 2025-07-04 17:22:31
tags:
  - zabbix_agent
categories:
  - Zabbix
---

# Zabbix 被监控端自动被采集

### 修改被监控端的配置文件（通常位于 /etc/zabbix/zabbix_agentd.conf）：

```bash
Server=192.168.1.100       # 允许Server拉取数据
ServerActive=192.168.1.100 # 允许主动上报到Server
Hostname=Your_Host_Name    # 关键！与Zabbix Web中的主机名一致
```

### 重启 zabbix-agent 服务

```bash
systemctl restart zabbix-agent
```

### 在 Zabbix Web 中添加主机：

登录 Zabbix Web → Configuration → Hosts → Create Host。

填写与配置文件一致的 Hostname。

在 Interfaces 中添加 Agent 的 IP 和端口（默认 10050）。

链接模板（如 Template OS Linux）。

### 检查通信：

在 Zabbix Web 中查看主机的 Availability 列（绿灯表示正常）。

通过日志排查问题：

```bash
tail -f /var/log/zabbix/zabbix_agentd.log
```


### 常见问题
Hostname不匹配：配置文件中的 Hostname 必须与Zabbix Web中的主机名完全一致。

防火墙/网络问题：确保10050（Agent端口）和10051（Server端口）开放。

主动模式 vs 被动模式：

- 被动模式：Server主动拉取Agent数据（需配置 Server）。

- 主动模式：Agent主动上报数据到Server（需配置 ServerActive）。

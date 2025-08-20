----
title: OSI
date: 2025-05-12 19:45:13
tags:
  - OSI
  - 网络
categories: 网络
description: "OSI 七层模型"
---

# 🌐 总结对照表：

| 层级 | 名称       | 常见协议举例                         |
| ---- | ---------- | ------------------------------------ |
| 7    | 应用层     | HTTP, FTP, SMTP, DNS, SSH            |
| 6    | 表示层     | TLS/SSL, JPEG, MPEG, GZIP            |
| 5    | 会话层     | NetBIOS, RPC, SMB                    |
| 4    | 传输层     | TCP, UDP, SCTP                       |
| 3    | 网络层     | IP, ICMP, IGMP, IPSec                |
| 2    | 数据链路层 | Ethernet, ARP, PPP, VLAN             |
| 1    | 物理层     | RJ-45, IEEE 802.11, 光纤, 同轴电缆等 |

## 🧱 1. 物理层（Physical Layer）

> 作用：传输比特流（0 和 1），涉及硬件设备与物理介质
> **常见协议/标准：**

- Ethernet（以太网物理标准，如 100Base-T）
- IEEE 802.11（WiFi 物理标准）
- 光纤、同轴电缆等
- RS-232（串行通信标准）

## 🌐 2. 数据链路层（Data Link Layer）

> 作用：在物理层上传输帧，处理 MAC 地址、差错检测等
> **常见协议/标准：**

- Ethernet（以太网帧封装）
- ARP（地址解析协议）
- PPP（点到点协议）
- HDLC（High-Level Data Link Control）

## 🌐 3. 网络层（Network Layer）

> 作用：负责路由选择与 IP 寻址，实现跨网段通信
> **常见协议/标准：**

- IP（Internet Protocol，包括 IPv4、IPv6）
- ICMP（Internet Control Message Protocol，ping 用的）
- IGMP（Internet Group Management Protocol）
- IPSec（用于安全传输的 IP 加密协议）

## 🌐 4. 传输层（Transport Layer）

> 作用：提供可靠的数据传输，确保数据包按序到达
> **常见协议/标准：**

- TCP（Transmission Control Protocol，可靠传输）
- UDP（User Datagram Protocol）– 无连接、不可靠但高效
- SCTP（Stream Control Transmission Protocol）

## 🌐 5. 会话层（Session Layer）

- 作用：建立、管理和终止会话
  **常见协议/标准：**
- NetBIOS（网络基本输入输出系统）
- RPC（远程过程调用）
- SMB（服务器消息块）
  > 注：现代网络中会话层通常和表示层、应用层一起实现。

## 🌐 6. 表示层（Presentation Layer）

> 作用：数据格式转换、加解密、压缩
> **常见协议/标准：**

- TLS/SSL（传输层安全协议）
- JPEG（联合照片专家组）
- MPEG（运动图像专家组）
- GZIP、DEFLATE（压缩算法）

## 🌍 7. 应用层（Application Layer）

> 作用：面向用户，提供各种网络服务的接口
> **常见协议/标准：**

- HTTP（超文本传输协议）
- FTP（文件传输协议）
- SMTP（简单邮件传输协议）
- DNS（域名系统）
- SSH（安全外壳协议）
  > 注：应用层协议通常是网络服务的入口，如 HTTP、FTP、SMTP、DNS 等。

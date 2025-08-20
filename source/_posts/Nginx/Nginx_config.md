---
title: Nginx 一些配置介绍
date: 2025-08-01 14:12:40
categories:
  - nginx
---

### Nginx 一些配置介绍

#### gzip 介绍

开启 gzip 后，Nginx 会在将响应返回给客户端前，对响应内容进行 压缩处理。这样可以减小数据传输体积，提升访问速度，尤其在移动网络下更明显。下面我详细说明其 工作机制 和相关参数：

```conf
gzip on;  # 启用 gzip 压缩

gzip_min_length 1024;  # 最小压缩大小（小于此值的内容不会压缩）

gzip_comp_level 5;  # 压缩等级（1-9，越高压缩率越好但 CPU 占用越高，推荐 4~6）

gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;  # 指定哪些类型的响应启用 gzip

gzip_disable "MSIE [1-6]\.";  # 禁用旧版 IE 浏览器的 gzip（它们支持不好）

gzip_vary on;  # 告诉缓存服务器保存 gzip 和非 gzip 两种版本（设置响应头：Vary: Accept-Encoding）
```

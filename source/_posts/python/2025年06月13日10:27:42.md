---
title: Python 取出两个数值
date: 2025-06-13 10:27:42
tags:
  - Python
categories:
  - Python
---

### 字典中取出 key 和 value

```python
config = {
    "DIFFNET": "192.168.1.1",
    "ROUTER": "192.168.1.254"
}

for key, value in config.items():
    print(f"{key} = {value}")
```

### 从列表中取出两个字段组成的元组

```python
config_list = [("DIFFNET", "192.168.1.1"), ("ROUTER", "192.168.1.254")]

for name, ip in config_list:
    print(f"{name} has IP {ip}")
```

### 从带有结构的配置文本中解析两个值

```text
DIFFNET=192.168.1.1
ROUTER=192.168.1.254
```

```python
with open("data.txt", "r") as f:
    for line in f:
        if "=" in line:
            key, value = line.strip().split("=")
            print(f"Key: {key}, Value: {value}")
```

### zip() 函数 两个列表组合成一个字典

```python
names = ["DIFFNET", "ROUTER"]
ips = ["192.168.1.1", "192.168.1.254"]

for name, ip in zip(names, ips):
    print(f"{name}: {ip}")
```

```python
names = ["DIFFNET", "ROUTER"]
ips = ["192.168.1.1", "192.168.1.254"]
data = {}
for name, ip in zip(names, ips):
    data[name] = ip
print(data)
```

---
title: Python 读取文件
date: 2025-06-30 16:19:59
tags:
  - Python
  - 文件
categories:
  - Python
---

# 一行一行的读取文件的三个方法

### ✅ 方法一：使用 with open() 和 for line in file（推荐）

```python
with open('filename.txt', 'r', encoding='utf-8') as f:
    for line in f:
        print(line.strip())  # 去除行尾换行符
```

- with 会自动关闭文件。

- line.strip() 用于去掉开头和结尾的空格、换行。

### ✅ 方法二：使用 readline()

```python
f = open('filename.txt', 'r', encoding='utf-8')
while True:
    line = f.readline()
    if not line:
        break
    print(line.strip())
f.close()

import json

f = open('config', 'r', encoding='utf-8')
result = {}
while True:
    line = f.readline()
    if "=" in line:
        key, value = line.split("=", 1) # 1  代表的是分割次数
        value = value.strip('"').strip("'").strip("\n").strip("\\") # 用两个strip来去除两边的空格和双引号
        result[key] = value
    if not line:
        break
    # print(line.strip())
f.close() # 关闭文件是为了释放内存
result = json.dumps(result, indent=4, ensure_ascii=False)
print(result)
```

**适合处理大文件，但要记得手动 close()。**

### ✅ 方法三：使用 readlines()

```python
with open('filename.txt', 'r', encoding='utf-8') as f:
    lines = f.readlines()

for line in lines:
    print(line.strip())
```

> ⚠️ 注意：如果文件很大，占用内存会比前两种方法高。

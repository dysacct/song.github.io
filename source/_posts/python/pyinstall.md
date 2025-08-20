---
title: Python打包工具
date: 2025-07-30 11:31:58
tags:
  - 打包exe
categories:
  - Python
---

## pyinstaller

```bash
pip install pyinstaller
```

```bash

# 存放路径
# C:\Users\123\Desktop\pyinstaller
cd C:\Users\123\Desktop\test

pyinstaller -F -i <图标的文件名.ico> --add-data "<图标文件夹>;<图标文件夹>" --add-data "<音乐文件夹>;<音乐文件夹>" <主程序.py>

```

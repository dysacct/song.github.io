---
title: Linux tar包解压锁
date: 2025-06-30 14:29:10
tags:
  - Linux
  - 命令
categories:
  - Linux
---

# Linux tar 包解压缩

|   文件后缀   |        解压命令        |              打包命令               |
| :----------: | :--------------------: | :---------------------------------: |
|     .tar     |   tar -xvf file.tar    |   tar -cvf file.tar dir_or_files    |
| .tar.gz/.tgz | tar -xzvf file.tar.gz  | tar -czvf file.tar.gz dir_or_files  |
|   .tar.bz2   | tar -xjvf file.tar.bz2 | tar -cjvf file.tar.bz2 dir_or_files |
|     .zip     |     unzip file.zip     |    zip -r file.zip dir_or_files     |

```bash

```

##  参数解释
**tar -xzvf** 
- x：extract（解包）
- z：gzip（解压缩）
- v：verbose（显示过程）
- f：file（指定文件名）

```bash
tar -xzvf file.tar.gz
```

```bash
tar -czvf archive.tar.gz 要打包的目录或文件

# 打包当前目录下的 myfolder 为 myfolder.tar.gz
tar -czvf myfolder.tar.gz myfolder

# 打包多个文件为 one.tar.gz
tar -czvf one.tar.gz file1.txt file2.sh dir3
```

## 总结解压命令尝试顺序
```bash
file filename.tar       # 先判断类型

# 根据类型决定：
tar -xvf filename.tar        # 纯 tar
tar -xzvf filename.tar       # tar.gz
tar -xjvf filename.tar       # tar.bz2
tar -xJvf filename.tar       # tar.xz
unzip filename.tar           # 可能是 zip
7z x filename.tar            # 万能工具
```
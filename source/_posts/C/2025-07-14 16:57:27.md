---
title: C语言字符串乱码
date: 2025-07-14 16:57:27
tags:
  - C乱码
categories:
  - C
---

UTF-8 万国码 中文占 3 个字节
GBK 中国区的编码 中文占 2 个字节
如果想要所有文字 需要都是 utf-8 的编码

```c
#include <stdio.h>

int main() {
  char* p = "我要努力学会C语言문자열\n";

  printf("%s", p);
  return 0;
}
```

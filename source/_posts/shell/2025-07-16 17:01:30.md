---
title: shellcheck
date: 2025-07-16 17:01:30
tags:
  - shell的静态分析工具
categories:
  - shell
---

# shellcheck

静态分析工具

### shellcheck disable=SC2317,SC2162

是用于禁用 ShellCheck 静态分析工具对特定规则（SC2317 和 SC2162）的警告的注释指令。

- ShellCheck 是一个用于检查 Shell 脚本语法错误、潜在问题和代码风格的工具。
- 它会根据预设的规则（如 SC2001、SC2162 等）提示代码中的风险或改进建议。

```bash
# shellcheck disable=SC2162
read -p "Enter name: " name  # 忽略 SC2162 的警告
```

### disable=SC2086

SC2086：双引号引用变量

```bash
# 错误写法（会触发 SC2086）：
echo $var

# 正确写法：
echo "$var"
```

### SC2155

SC2155：不要在声明和赋值中同时使用命令替换

```bash
# 错误写法（会触发 SC2155）：
local file=$(ls -l)

# 正确写法：
local file
file=$(ls -l)
```

### disable=SC1009

SC1009：if 等语句语法错误

```bash
# 错误写法（会触发 SC1009）：
if [ "$x" = "1" ]
echo "x is 1"

# 正确写法：
if [ "$x" = "1" ]; then
  echo "x is 1"
fi
```

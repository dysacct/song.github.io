---
title: shell 一些脚本片段
date: 2025-07-20 10:51:26
tags:
  - 脚本示例
categories:
  - shell
---

> 用户交互性例子

```bash

#!/bin/bash
# shellcheck disable=SC2162

echo "请输入一些选项（用空格分隔）：" # echo "请输入一些选项（用空格分隔）："
read -r options # options是一个数组
PS3="请选择一个选项： " # PS3是一个变量，用于提示用户输入
select choice in $options "Exit"
do
    if [ "$choice" = "Exit" ]; then
        echo "退出程序"
        break
    elif [ -n "$choice" ]; then
        echo "你选择了：$choice"
    else
        echo "无效选择，请输入有效编号！"
    fi
done


# =======================

#!/bin/bash
PS3="请选择一个水果（输入编号）： "  # 自定义提示符
select fruit in 苹果 香蕉 橙子 退出
do
    case $fruit in
        苹果)
            echo "你选择了苹果，价格：5元/斤"
            ;;
        香蕉)
            echo "你选择了香蕉，价格：3元/斤"
            ;;
        橙子)
            echo "你选择了橙子，价格：4元/斤"
            ;;
        退出)
            echo "退出程序"
            break  # 退出 select 循环
            ;;
        *)
            echo "无效选择，请输入 1 到 4 之间的编号！"
            ;;
    esac
done
```

> 判断机器的系统类型

```bash
__main() {
    read id1 id2 < <(grep '^ID' /etc/os-release | head -n 2 | awk -F= '{gsub(/"/,""); print $2}' | tr '[:upper:]' '[:lower:]' | tr '\n' ' ')

    if [[ "$id1" == "$id2" ]]; then
      echo "两者相等"
    else
      echo "错误：不相等. id1=$id1, id2=$id2"
      exit 1
    fi

}

bash<<<"$(declare -f __main | sed '$a __main')"

```

> 颜色输出

```bash
#!/bin/bash
out_success() {
    echo -e "[\033[32mSuccess\033[0m] $1"
}
out_warning() {
    echo -e "[\033[33mWarning\033[0m] $1"
}
out_error() {
    echo -e "[ \033[35mError\033[0m ] $1"
}
out_info() {
    echo -e "[ \033[36mInfo\033[0m ] $1"
}


__main() {

    out_info "这是蓝色的"
    out_success "这是绿色的"
    out_warning "这是黄色的"
    out_error "这是紫色的"
}

__main

```

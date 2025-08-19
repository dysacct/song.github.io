---
title: python 虚拟环境配置
date: 2025-08-19 17:51:50
categories:
  - Python
---

## 概述

### 示例

```bash
# 1. 创建项目目录
mkdir my_project
cd my_project

# 2. 创建虚拟环境
python3 -m venv venv

# 3. 激活环境
source venv/bin/activate

# 4. 升级pip
pip install --upgrade pip

# 5. 安装需要的包
pip install requests

# 6. 保存依赖列表
pip freeze > requirements.txt

# 7. 清除缓存
pip cache purge

# 8. 工作完成后退出
deactivate


```

添加永久镜像源
```bash
# 创建配置目录
mkdir -p ~/.pip

# 创建配置文件
cat > ~/.pip/pip.conf << EOF
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/
trusted-host = pypi.tuna.tsinghua.edu.cn
EOF
```
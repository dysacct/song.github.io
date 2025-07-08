---
title: Python的API
date: 2025-07-04 11:00:22
tags:
  - 创建FastAPI
categories:
  - Python
---

# Python 快速搭建 API 接口

```bash
pip install fastapi
```

```python
from fastapi import FastAPI
import random

app = FastAPI()

@app.get('/')  # 制定获取信息的路径
# 需要创建一个异步函数
async def root(): # 这是根路径，每次加载API时,这是第一个响应的
    return {'example': "this is an example", 'data': 0}  # 返回一个json或者字典都可以，这个包可以自动把字典转换成json

```

> 需要使用 unicorn 工具启动
```bash
uvicorn main:app --reload
```
main.py 是文件名，app 是变量名， --reload 是热更新，每次修改代码后，不需要重启服务，直接刷新页面即可Q

---
title: Python和shell处理json格式的文件
date: 2025-07-04 10:27:40
tags:
  - json
categories:
  - Python
---

# 处理 json 文件

```bash
{
  "rules": [
    {
      "id": 1,
      "conditions": [
        { "name": "mode_a", "value": true },
        { "name": "user_logged_in", "value": false }
      ]
    },
    {
      "id": 2,
      "conditions": [
        { "name": "mode_b", "value": true }
      ]
    }
  ]
}
```

### Python

```python
import json

json_str = "   "
# 或者直接处理文件
with open('data.json', 'r', encoding='utf-8') as f:
    data = json.load(f)
data = json.loads(json_str)

# 提取所有条件里的 name
names = []
for rule in data['rules']:
    for condition in rule.get('conditions', []):
        names.append(condition['name'])

print("提取到的 name：", names)
```

#### 🧠 小结（如何逐层取值）

从结构上讲，你需要逐层“下钻”：

- data["rules"] 是一个列表 ⇒ 遍历它。

- 每个元素是个字典，里面有 "conditions" ⇒ 继续遍历这个列表。

- 每个 condition 是个字典 ⇒ 取出 "name"。

这就是典型的 嵌套列表+字典 结构处理方式。

### Shell

```bash
jq -r '.rules[].conditions[].name' example.json
```

### 🧱 JSON 结构图

```bash
root
└── rules [list]
    ├── 0 [dict]
    │   ├── id: 1
    │   └── conditions [list]
    │       ├── 0 [dict]
    │       │   ├── name: "mode_a"
    │       │   └── value: true
    │       └── 1 [dict]
    │           ├── name: "user_logged_in"
    │           └── value: false
    └── 1 [dict]
        ├── id: 2
        └── conditions [list]
            └── 0 [dict]
                ├── name: "mode_b"
                └── value: true
```

---
title: json 解析
date: 2025-07-24 09:35:37
tags:
  -
categories:
  - Golang
---

## 解析 json 数据
```go
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
  jsonData := `{
    "status": "success",
    "timestamp": "2025-07-21T10:00:00Z",
    "data": [
      {
        "host": "server-01",
        "metrics": {
          "cpu": {
            "usage": 35.6,
            "cores": 4
          },
          "memory": {
            "total": 32768,
            "used": 16384,
            "unit": "MB"
          },
          "disk": [
            {
              "mount": "/",
              "total": 512000,
              "used": 400000,
              "unit": "MB"
            }
          ],
          "network": {
            "eth0": {
              "rx_bytes": 123456789,
              "tx_bytes": 987654321
            }
          }
        }
      }
    ]
  }`
  var result map[string]interface{}
  err := json.Unmarshal([]byte(jsonData), &result)
  if err != nil {
    fmt.Println("Error:", err)
    return
  }
  fmt.Println("Result:", result)
}

```
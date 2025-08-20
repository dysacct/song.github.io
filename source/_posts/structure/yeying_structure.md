---
title: 夜莺架构
date: 2025-08-11 11:40:29
categories:
  - 夜莺
---

# Linux服务器监控系统

一个基于容器化的Linux服务器监控解决方案，使用VictoriaMetrics作为时序数据库，夜莺(Nightingale)作为监控平台。

## 🎯 项目目标

- 🖥️ **多服务器监控**: 支持大规模Linux服务器集群监控
- 📊 **自定义指标**: 支持自定义业务指标采集和上传
- 🔄 **容器化部署**: 通过Docker容器快速部署到各个服务器
- 📈 **可视化展示**: 通过夜莺平台进行数据可视化和告警
- 🚀 **高性能存储**: 使用VictoriaMetrics提供高性能时序数据存储

## 🏗️ 系统架构

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Linux服务器1   │    │   Linux服务器2   │    │   Linux服务器N   │
│                │    │                │    │                │
│ ┌─────────────┐ │    │ ┌─────────────┐ │    │ ┌─────────────┐ │
│ │监控代理容器  │ │    │ │监控代理容器  │ │    │ │监控代理容器  │ │
│ │- node-exp   │ │    │ │- node-exp   │ │    │ │- node-exp   │ │
│ │- vmagent    │ │    │ │- vmagent    │ │    │ │- vmagent    │ │
│ │- custom     │ │    │ │- custom     │ │    │ │- custom     │ │
│ └─────────────┘ │    │ └─────────────┘ │    │ └─────────────┘ │
└─────────┬───────┘    └─────────┬───────┘    └─────────┬───────┘
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 │
                    ┌─────────────▼─────────────┐
                    │    VictoriaMetrics       │
                    │      时序数据库           │
                    └─────────────┬─────────────┘
                                 │
                    ┌─────────────▼─────────────┐
                    │      夜莺(Nightingale)    │
                    │    监控平台 & 可视化       │
                    └───────────────────────────┘
```

## 📦 项目结构

```
shell/
├── monitoring-agent/                 # 监控代理配置
│   ├── docker-compose-agent.yaml    # 代理容器编排文件
│   └── entrypoint.sh                # 自定义指标采集脚本
├── prometheus.yml                   # Prometheus配置文件
├── docker-compose-vm.yaml          # VictoriaMetrics服务器配置
├── deploy_monitoring.sh            # 一键部署脚本
├── nightingale-config.md           # 夜莺配置指南
└── README.md                       # 项目说明文档
```

## 🚀 快速开始

### 1. 部署VictoriaMetrics服务器

在您的中央服务器上部署VictoriaMetrics：

```bash
# 克隆项目
git clone <your-repo>
cd shell

# 修改配置
vim docker-compose-vm.yaml  # 修改端口和存储配置

# 启动VictoriaMetrics服务器
docker compose -f docker-compose-vm.yaml up -d

# 验证部署
curl http://localhost:8428/api/v1/label/__name__/values
```

### 2. 部署监控代理到Linux服务器

在每台需要监控的Linux服务器上运行：

```bash
# 下载部署脚本
wget https://your-server/deploy_monitoring.sh
chmod +x deploy_monitoring.sh

# 修改VictoriaMetrics服务器地址
vim deploy_monitoring.sh  # 修改VM_URL变量

# 一键部署监控代理
./deploy_monitoring.sh
```

### 3. 配置夜莺数据源

参考 `nightingale-config.md` 文档配置夜莺：

```bash
# 在夜莺中添加数据源
# URL: http://your-vm-server:8428
# 类型: Prometheus
```

## 📊 自定义指标上传

### 方法1: 使用脚本上传

```bash
# 上传CPU负载指标
/opt/monitoring/scripts/upload_custom_metrics.sh m_1010_cpu_load 1.25 'type="1min"'

# 上传内存使用率
/opt/monitoring/scripts/upload_custom_metrics.sh m_1010_memory_usage 75.5

# 上传磁盘使用率  
/opt/monitoring/scripts/upload_custom_metrics.sh m_1010_disk_usage 45 'mount="/"'
```

### 方法2: 直接API调用

```bash
# 构造指标数据
HOSTNAME=$(hostname)
TIMESTAMP=$(date +%s)000
METRIC_VALUE="1.25"

# 上传到VictoriaMetrics
curl -X POST "http://vm-server:8428/api/v1/import/prometheus" \
    -H "Content-Type: text/plain" \
    -d "m_1010_cpu_load{hostname=\"$HOSTNAME\",type=\"1min\"} $METRIC_VALUE $TIMESTAMP"
```

### 方法3: 程序中集成

```python
import requests
import time
import socket

def upload_metric(metric_name, value, labels=None):
    hostname = socket.gethostname()
    timestamp = int(time.time() * 1000)
    
    if labels:
        label_str = f'hostname="{hostname}",' + labels
    else:
        label_str = f'hostname="{hostname}"'
    
    metric_line = f'{metric_name}{{{label_str}}} {value} {timestamp}'
    
    response = requests.post(
        'http://vm-server:8428/api/v1/import/prometheus',
        headers={'Content-Type': 'text/plain'},
        data=metric_line
    )
    return response.status_code == 204

# 使用示例
upload_metric('m_1010_cpu_load', 1.25, 'type="1min"')
```

## 🔍 数据查询

### PromQL查询示例

```promql
# 查询CPU负载
m_1010_cpu_load{type="1min"}

# 查询特定主机的内存使用率
m_1010_memory_usage{hostname="server1"}

# 查询磁盘使用率超过80%的主机
m_1010_disk_usage > 80

# 计算平均CPU负载
avg(m_1010_cpu_load{type="1min"})

# 查询过去1小时的CPU负载趋势
m_1010_cpu_load{type="1min"}[1h]
```

### API查询示例

```bash
# 查询当前CPU负载
curl -G "http://vm-server:8428/api/v1/query" \
    --data-urlencode "query=m_1010_cpu_load{type=\"1min\"}"

# 查询时间范围数据
curl -G "http://vm-server:8428/api/v1/query_range" \
    --data-urlencode "query=m_1010_cpu_load{type=\"1min\"}" \
    --data-urlencode "start=$(date -d '1 hour ago' +%s)" \
    --data-urlencode "end=$(date +%s)" \
    --data-urlencode "step=60"
```

## 🎛️ 夜莺可视化

### 1. 添加数据源
- 进入夜莺管理界面
- 系统管理 -> 数据源管理 -> 新增数据源
- URL: `http://your-vm-server:8428`
- 类型: `Prometheus`

### 2. 创建仪表盘
- 仪表盘管理 -> 新建仪表盘
- 添加Panel，使用上述PromQL查询语句

### 3. 配置告警规则
- 告警管理 -> 新建规则
- 查询语句: `m_1010_cpu_load{type="1min"} > 2`
- 持续时间: `5m`

## 🔧 高级配置

### 自定义指标规范

建议的指标命名规范：
- `m_<业务编号>_<指标名>`
- 示例: `m_1010_cpu_load`, `m_2020_api_response_time`

### 标签规范

推荐的标签：
- `hostname`: 主机名
- `type`: 指标类型
- `mount`: 挂载点（磁盘相关）
- `interface`: 网络接口（网络相关）

### 性能优化

1. **数据保留策略**: 修改VictoriaMetrics的`retentionPeriod`
2. **采集频率**: 调整`prometheus.yml`中的`scrape_interval`
3. **内存限制**: 设置VictoriaMetrics的内存使用限制

## 🐛 故障排查

### 常见问题

1. **指标无法上传**
   ```bash
   # 检查VictoriaMetrics连接
   curl http://vm-server:8428/api/v1/query?query=up
   
   # 检查容器状态
   docker ps | grep victoria
   ```

2. **夜莺无法连接数据源**
   ```bash
   # 检查网络连通性
   telnet vm-server 8428
   
   # 检查防火墙设置
   iptables -L | grep 8428
   ```

3. **自定义指标丢失**
   ```bash
   # 查看指标列表
   curl http://vm-server:8428/api/v1/label/__name__/values | grep m_1010
   ```

### 日志查看

```bash
# 查看VictoriaMetrics日志
docker logs victoriametrics

# 查看监控代理日志
docker logs custom-metrics-collector

# 查看vmagent日志
docker logs vmagent
```

## 📈 性能指标

- **数据压缩率**: VictoriaMetrics提供20:1的压缩比
- **查询性能**: 支持百万级时间序列的秒级查询
- **存储效率**: 比Prometheus节省50%以上存储空间
- **内存使用**: 比Prometheus减少70%内存使用

## 🤝 贡献指南

1. Fork 项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送分支 (`git push origin feature/AmazingFeature`)
5. 提交Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情

## 📞 支持与联系

- 项目问题: [提交Issue](https://github.com/your-repo/issues)
- 技术讨论: [加入讨论](https://github.com/your-repo/discussions)
- 邮件联系: admin@yourdomain.com

---

## 🎉 更新日志

### v1.0.0 (2024-01-15)
- ✨ 初始版本发布
- 🚀 支持VictoriaMetrics时序数据库
- 📊 集成夜莺监控平台
- 🐳 完整的容器化部署方案
- 📝 详细的配置文档和使用指南
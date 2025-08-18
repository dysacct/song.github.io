---
title: crontab 定时任务
date: 2025-07-22 10:16:37
tags:
  - 定时任务
categories:
  - linux
---

## crontab 设置定时任务

```bash
crontab -l # 查看当前用户的定时任务

crontab -e # 编辑定时任务
```

```cron
# 每分钟执行一次：
* * * * * /path/to/script.sh
# 所有时间字段都是 *，表示每分钟运行。每天凌晨 2:30 执行：
30 2 * * * /path/to/backup.sh
# 分钟=30，小时=2，其他字段任意。
# 每周一 8:00 执行：
0 8 * * 1 /path/to/weekly_task.sh
# 分钟=0，小时=8，星期=1（周一）。
# 每 10 分钟执行一次：
*/10 * * * * /path/to/check_status.sh
# 分钟字段 */10 表示每 10 分钟。
# 每月 1 号和 15 号的 9:00 执行：
0 9 1,15 * * /path/to/monthly_report.sh
# 日期字段 1,15 表示每月 1 号和 15 号。
# 工作日的每天下午 3:00 执行：
0 15 * * 1-5 /path/to/workday_task.sh
# 星期字段 1-5 表示周一到周五。
# 每小时的第 0 分钟和第 30 分钟执行：
0,30 * * * * /path/to/half_hour_task.sh
分钟字段 0,30 表示每小时的第 0 分钟和第 30 分钟。
```

```cron
* * * * * root command-to-execute
| | | | | |
| | | | | +--- 用户名
| | | | +----- 星期几 (0 - 7)（0 和 7 都表示星期天）
| | | +------- 月份 (1 - 12)
| | +--------- 日期 (1 - 31)
| +----------- 小时 (0 - 23)
+------------- 分钟 (0 - 59)
```

```bash
#!/bin/bash

# 配置项
DB_USER="root"
DB_PASSWORD="你的密码"
DB_NAME="你的数据库名"
BACKUP_DIR="/data/mysql_backup"

# 日期格式
DATE=$(date +"%Y-%m-%d")
FILENAME="${DB_NAME}_${DATE}.sql"
TARFILE="${FILENAME}.tar.gz"

# 创建备份目录（如果不存在）
mkdir -p "$BACKUP_DIR"

# 执行备份
mysqldump -u"$DB_USER" -p"$DB_PASSWORD" "$DB_NAME" > "$BACKUP_DIR/$FILENAME"

# 压缩备份文件
tar -czf "$BACKUP_DIR/$TARFILE" -C "$BACKUP_DIR" "$FILENAME"

# 删除原始 .sql 文件
rm -f "$BACKUP_DIR/$FILENAME"

# 删除7天前的备份
find "$BACKUP_DIR" -type f -name "*.tar.gz" -mtime +7 -exec rm -f {} \;

echo "[`date`] MySQL 备份完成: $TARFILE"

```

```ini
[client]
user=root
password=你的密码
```

chmod 600 ~/.my.cnf

> 然后脚本里就可以直接使用

```bash
mysqldump "$DB_NAME" > "$BACKUP_DIR/$FILENAME"
```

chmod +x /usr/local/bin/mysql_backup.sh

```cron
0 2 * * * /usr/local/bin/mysql_backup.sh >> /var/log/mysql_backup.log 2>&1
```

> MySQL 客户端程序（如 mysql, mysqldump）会自动查找并读取默认配置文件路径中的
> .my.cnf，你不需要在脚本里手动“引用”

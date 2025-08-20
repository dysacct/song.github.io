---
title: MySQL 数据备份
date: 2025-06-30 15:01:53
tags:
  - MySQL
categories:
  - MySQL
---

# MySQL 数据备份

### 备份

```bash
# 备份整个数据库
mysqldump -u 用户名 -p 数据库名 > backup.sql
# 备份多个数据库
mysqldump -u 用户名 -p --databases 数据库1 数据库2 > multi_backup.sql
# 备份所有数据库
mysqldump -u 用户名 -p --all-databases > all_backup.sql
# 备份单个表
mysqldump -u 用户名 -p 数据库名 表名 > backup.sql
# 添加其他常用参数（更完整的备份）
mysqldump -u 用户名 -p --single-transaction --quick --lock-tables=false 数据库名 > backup.sql
```

- --single-transaction：适用于 InnoDB 表，确保数据一致性。

- --quick：快速导出大数据表。

- --lock-tables=false：避免锁表（InnoDB 不需要锁表即可保证一致性）。

### 使用 mysql 还原数据

```bash
# 还原到某个数据库
mysql -u 用户名 -p 数据库名 < backup.sql

# 💡 要确保数据库 mydb 事先已经存在。如果没有，你需要先创建：
mysql -u root -p -e "CREATE DATABASE mydb CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;"


# 还原包含 CREATE DATABASE 的备份文件
# 如果你的备份文件里有 CREATE DATABASE（例如使用了 --databases 或 --all-databases 参数），你可以直接用
mysql -u 用户名 -p < backup.sql
```

### Docker 容器环境

```bash
# 从容器中备份数据库：
docker exec -i 容器名 mysqldump -u用户名 -p密码 数据库名 > backup.sql

# 将备份恢复到容器中的 MySQL：
cat backup.sql | docker exec -i 容器名 mysql -u用户名 -p密码 数据库名

```

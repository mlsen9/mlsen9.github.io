---
title: MySQL 异常排查
date: 2022-02-13 11:53:33 +0800
categories: [Database, MySQL]
tags: [mysql-trouble, devops]
---

### 当前运行的所有事务

```sql
SELECT * FROM information_schema.INNODB_TRX;
```

### 查看超时链接
```sql
SHOW GLOBAL VARIABLES LIKE '%timeout';
```

### 查看最大连接数
```sql
SHOW GLOBAL VARIABLES LIKE '%MAX_CONNECTION%';
```

### 查看连接数（连接总数、活跃数、最大并发数）
```sql
SHOW GLOBAL STATUS LIKE 'THREAD%';
```
| Variable_name | Value | 说明 |
|---|---|---|
| Threads_connected  | 1  | 打开的连接数  |
| Threads_created    | 4  | 创建过的线程数  |
| Threads_running    | 1  | 激活的连接数  |


### 查看所有用户连接
```sql
SHOW FULL PROCESSLIST;
```

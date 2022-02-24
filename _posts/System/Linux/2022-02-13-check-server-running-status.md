---
title: 检查服务运行状态
date: 2022-02-13 11:53:33 +0800
categories: [System, Linux]
tags: [linux-commands, devops]
---

## 检查服务是否正常运行及重启方法

### 查看 php-cgi/php-fpm 进程数

```bash
netstat -anpo | grep "php-fpm" | wc -l
```

### 查询服务器并发数
```bash
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a,S[a]}'
```

### 查看 php-fpm 9000 端口是否已监听
```bash
netstat -tpnl | grep 9000
```

### 查看 mysqld 3306 端口
```bash
netstat -tpnl | grep 3306
```

### 查看 memcache 缓存服务 11211 端口
```bash
netstat -tpnl | grep 11211
```

### 查看 redis 缓存服务 6392 端口
```bash
netstat -tpnl | grep 6392
```

### 查看 nginx 服务 80 端口
```bash
netstat -tpnl | grep 80
```

### 查看 mongodb 服务 27710 端口
```bash
netstat -tpnl | grep 27710
```

### 查看 nginx 进程是否存在
```bash
ps aux | grep "nginx"
```

### 查看 php-fpm 进程是否存在
```bash
ps aux | grep "php-fpm"
```

### 查看 memcache 进程是否存在
```bash
ps aux | grep "memcached"
```

### 查看 redis 进程是否存在
```bash
ps aux | grep "redis"
```

### 查看 mongodb 进程是否存在
```bash
ps aux | grep "mongod"
```

### nginx 服务重启
```bash
/etc/init.d/nginx restart
```

### php-fpm 服务重启
```bash
/etc/init.d/php-fpm restart
```

### memcache 服务重启
```bash
/usr/local/memcached/bin/memcached -d -l 127.0.0.1 -p 11211 -u root -m 64 -c 1024 -P /var/run/memcached.pid
```

### redis 服务重启
```bash
/data/apps/redis/src/redis-server 127.0.0.1:6392
```

### mongodb 服务重启
```bash
/usr/bin/mongod --quiet -f /etc/mongod.conf run
```

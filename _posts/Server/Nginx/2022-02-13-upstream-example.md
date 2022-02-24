---
title: Nginx 负载均衡配置示例
date: 2022-02-13 11:53:33 +0800
categories: [Server, Nginx]
tags: [nginx-conf, nginx-upstream]
---

### 定义前端负载

```nginx
# 定义负载web服务器池
upstream mysvr {
    # weigth参数表示权值，权值越高被分配到的几率越大
    server 172.17.0.2:82  weight=100;
    # 利用容器别名（需提前绑定 --link nginx82:nginx82）
    #server nginx82:82  weight=100;
}

server {
    listen       81;
    server_name  localhost;

    #日志
    error_log   /home/logs/nginx.error.log   error;
    access_log  /home/logs/nginx.access.log  main;

    location / {
        proxy_pass http://mysvr;

        ## 有代理情况下，设置用户真实IP
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Real-Port $remote_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

### 后端web项目

```nginx
server {
    listen       82;
    server_name  localhost;

    #日志
    error_log   /home/logs/nginx.error.log   error;
    access_log  /home/logs/nginx.access.log  main;

    location / {
        root   /home/www/project;
        index  index.php;
    }

    location ~ \.php$ {
        root           /home/www/project;
        fastcgi_pass   127.0.0.1:9000;
        # 利用容器别名（需提前绑定 --link php72:php72）
        #fastcgi_pass   php72:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }

    # redirect server error pages to the static page /50x.html
    error_page  500 502 503 504  /50x.html;
    location = /50x.html {
        root  /usr/share/nginx/html;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    location ~ /\.ht {
        deny  all;
    }
}
```

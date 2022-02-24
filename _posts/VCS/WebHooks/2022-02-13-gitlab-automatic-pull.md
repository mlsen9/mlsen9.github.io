---
title: 利用WebHooks实现代码自动拉取教程
date: 2022-02-13 11:53:33 +0800
categories: [VCS, WebHooks]
tags: [git, gitlab, webhooks]
toc: true
---

其原理是利用 `GitLab/GitHub/YourRepository` 提供的 `WebHooks 钩子事件`，创建一个钩子并且监听开发者的 `push事件`，接收到push事件后会通过POST请求将事件信息推送到指定的URL。当自行搭建的Nginx服务器接收到这个请求后，转发到自己写的 `PHP脚本`，PHP脚本会做一些基础校验（例如：访问令牌、密码），然后执行提前写好的 `Shell `脚本来拉取代码。

### 准备工作

> 需要 `Nginx + PHP 7.* + Swoole 扩展` 的环境支持
{: .prompt-info }


#### 添加钩子说明：
需要填写【URL（key 用于区分项目）、Secret Token、Trigger】

**URL填写：**
`http://test.local.com/gitlab_webhooks?key=c40d160c2ffa013b`
**Secret Token填写：**1234567890
**Trigger填写：**仅勾选 Push events 即可


```
假设项目目录为：
/home/wwwroot/test.local.com
/home/shells/git_pull.sh
Nginx配置文件：
/usr/local/nginx/conf/vhosts/test.local.com.conf
访问项目网址为：
http://test.local.com
也可直接省略 Nginx 服务，直接采用 Swolle 监听 HTTP服务。
案例因为使用的是 Docker 容器，项目在容器内。并且只开放了80端口，所以需要 Nginx 做一次转发工作。
```

### 1. 修改配置文件，增加代理转发

`以下几步操作都需要进入到 linux 服务器里面进行操作`

```nginx
vim /usr/local/nginx/conf/vhosts/test.local.com.conf
# swoole 方式
## GitLab Webhooks 处理推送事件
location ^~ /gitlab_webhooks {
  ## 此处使用了 ^~ 匹配，所以符合这个条件的，将不再匹配下面的 localtion 段
  ## 转发到本机 6200 端口的 Swoole 程序处理
  proxy_pass http://127.0.0.1:6200;
}

## 在这个指令上面增加 代理转发
location / {}

-----

# php-fpm 方式
location ^~ /gitlab_webhooks {
    root /home/wwwroot/gitlab_webhooks;
    fastcgi_pass  unix:/tmp/php-cgi.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
    include       fastcgi_params;
}

## 在这个指令上面增加 代理转发
location / {}
```

### 2. 上传处理钩子的代码

 **主要的程序文件**

- [WebhooksInterface.php](https://gist.github.com/helloxxb/9f6cee5abf0fa98801787b4f28e47a40)
- [GitPull.php](https://gist.github.com/helloxxb/6270295e556ef98cd01a4942cde07f9b)
- [WebhooksHttpServer.php](https://gist.github.com/helloxxb/91b2750df0f1d43aac56e4d36acdbf04)


### 3. 启动 Swoole 程序

```bash
php /home/wwwroot/gitlab_webhooks/WebhooksHttpServer.php
此程序监听的是 127.0.0.1:6200
```

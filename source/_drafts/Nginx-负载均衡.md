---
title: Nginx 负载均衡
tags:
- nginx
---

# Nginx 介绍

> Nginx 是异步框架的网页服务器，也可以用作反向代理、负载平衡器和 HTTP 缓存。该软件由伊戈尔·赛索耶夫创建并于 2004 年首次公开发布。 2011 年成立同名公司以提供支持。2019 年 3 月 11 日，Nginx 公司被 F5 Networks 以 6.7 亿美元收购。 Nginx 是免费的开源软件，根据类 BSD 许可证的条款发布。 [维基百科](https://zh.wikipedia.org/zh-cn/Nginx)

## 安装

### 源码编译

（坑待填）

### Ubuntu 包管理

```sh
sudo apt install nginx
```

安装完成后有几个默认的目录需要了解一下：

```sh
 ~ tree -L 1 /etc/nginx/
/etc/nginx/
├── conf.d              # 配置文件存放的目录，可以定以不同的配置
├── fastcgi.conf
├── fastcgi_params
├── koi-utf
├── koi-win
├── mime.types
├── modules-available
├── modules-enabled
├── nginx.conf          # 默认的配置文件
├── scgi_params
├── sites-available
├── sites-enabled
├── snippets
├── uwsgi_params
└── win-utf

~ ls /usr/share/nginx/html/
50x.html    index.html # 默认显示的主页

~ ls /var/www/html/
index.nginx-debian.html # 另一个默认显示的主页
```

具体显示哪一个文件是由 Nginx 的配置文件 nginx.conf 决定的，查看一下默认的配置文件（以下内容取自 docker 容器，镜像：）。

```sh
# cat /etc/nginx/nginx.conf

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;  # error_log 的位置和日志级别
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;  # access_log 的位置和日志格式（上面 main 定义的）

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;  # 这里引入了 conf.d 目录下的全部以 conf 结尾的配置文件，可以给不同的功能写不同的配置文件，
                                       # 这样可以避免配置文件内容过多导致难以维护的问题。
}
```

nginx.conf 这个文件中并没有配置具体的 http 服务，查看 conf.d 目录下发现有一个名为 default.conf 的配置文件，内容如下：

```sh
# cat /etc/nginx/conf.d/default.conf

server {
    listen       80;  # 监听 80 端口
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {  # 定义了一个 URL, 在 root 标识的目录下寻找 index.html 或 index.htm 作为主页
        root   /usr/share/nginx/html;  # web 服务的根目录
        index  index.html index.htm;   # 主页的名称
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;  # 出现 50x 错误返回的页面
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {  ~ 表示开启正则匹配，这里的 URL 表示以 .php 结尾的文件
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {  # 阻止所有访问以 ht 结尾的文件的请求
    #    deny  all;
    #}
}

```

# 什么是负载均衡

# 使用 Nginx 进行负载均衡
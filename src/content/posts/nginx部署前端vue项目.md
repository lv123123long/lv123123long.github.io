---
title: ubuntu nginx 部署vue项目
pubDate: 2024-08-28
categories: ['blog']
description: ''
---

# 部署Vue项目

## Vue

vue脚本位置 package.json

```
pnpm build
```



端口修改：vue.config.js



将dist目录打包到服务器上面去，配置nginx相关配置



打包生成

## Ubuntu

安装nginx

```
sudo apt install nginx

```

查看版本

```
nginx -v

```

启动ngxin

```
service nginx start
```

```
service nginx stop
```

```
service nginx restart
```







## Nginx

1.在/ect/nginx下创建hosts文件：mkidr hosts

2.在下创建 xxx.host并对其进行编辑，内容如下：（一个vue打包项目对应一个host文件）

```
sudo vim /ect/nginx/hosts/xxx.host
```



```
server {
        listen       8080;#自己设置端口号

        server_name  seller_sprite;#自己设置项目名称

        #access_log  logs/host.access.log  main;

        location / {
            autoindex on;

            root   /home/ubuntu/vue/dist;#这里写vue打包项目(dist)的所在地址

            index  index.html;#这里是vue项目的首页，需要保证dist中有index.html文件

        }

        error_page   500 502 503 504  /50x.html;#错误页面

    }

```

3.修改/etc/nginx/nginx.conf配置文件，内容如下:

```
sudo vim /etc/nginx/nginx.conf
```



```
user ubuntu;
worker_processes auto;
pid /run/nginx.pid;
error_log /var/log/nginx/error.log;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        types_hash_max_size 2048;
        # server_tokens off;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##

        access_log /var/log/nginx/access.log;
        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
        include /etc/nginx/hosts/*.host;
}

```



* 修改启动用户，，vue打包的目录，这个用户需要有权限
* 最后一行的 include
* 重启nginx

报错日志

```
vim /var/log/nginx/error.log
```



## Django

将后端地址端口，改成 前端地址一样，是没有用的 

```
python manage.py runserver 0.0.0.0:8888
```



## 参考

https://blog.csdn.net/m0_62262778/article/details/137401867




---
title: web
pubDate: 2024-08-27
categories: ['blog']
description: ''
---

# web

## OverView

```
FROM nginx:1.25.3-alpine
# 这份是用于从本地的静态资源构建镜像的 Dockerfile

# 将 Nginx 配置文件模板拷到容器中
COPY default.conf.template /etc/nginx/conf.d/default.conf.template
COPY default.conf.ssl.template /etc/nginx/conf.d/default.conf.ssl.template

# 静态资源 拷到容器
ADD dist_blog/ /usr/share/nginx/html/
ADD dist_admin/ /usr/share/nginx/html/admin

# 初始化脚本, 根据环境变量和模板生成 Nginx 配置文件
COPY ./run.sh /docker-entrypoint.sh
RUN chmod a+x /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]

# 每次容器启动时执行
CMD [ "nginx", "-g", "daemon off;" ]

EXPOSE 80
EXPOSE 443
```

用于构建一个基于 Nginx 的 Docker 镜像，该镜像主要用于提供静态资源服务

指定了基础镜像为 `nginx:1.25.3-alpine`，即使用 Alpine Linux 作为基础，并安装了 Nginx 1.25.3 版本。

- `ENTRYPOINT ["/docker-entrypoint.sh"]`：指定容器启动时执行的脚本为 `/docker-entrypoint.sh`。
- CMD命令运行，启动 Nginx 并保持前台运行，这样容器不会因为 Nginx 后台运行而退出。
- 暴露端口，声明了容器将监听 80 和 443 端口，分别用于 HTTP 和 HTTPS 服务。
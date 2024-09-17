---
title: start
pubDate: 2024-09-17
categories: ['blog']
description: ''
---

# start启动脚本

## docker-compose.yml

Docker Compose文件，用来定义和运行多容器Docker应用程序



### 网络定义

```
networks:
  gvb-network:
    driver: bridge
    ipam:
      config:
        - subnet: ${SUBNET}

```

* 定义了一个名为gvb-network 的网络
* 使用bridge 驱动
* 使用ipam配置子网，子网地址由环境变量SUBNET指定



### gvb-redis

```
services:
  gvb-redis:
    image: redis:7.0-alpine
    container_name: gvb-redis
    restart: always
    volumes:
      - ${DATA_DIRECTORY}/data/redis/:/data
    ports:
      - ${REDIS_PORT}:6379
    command: redis-server --requirepass ${REDIS_PASSWORD} --appendonly yes
    networks:
      gvb-network:
        ipv4_address: ${REDIS_HOST}

```

- 使用`redis:7.0-alpine`镜像创建Redis容器。
- 容器名称为`gvb-redis`。
- 设置容器在退出时自动重启。
- 将主机上的数据目录挂载到容器的`/data`目录。
- 暴露Redis的默认端口6379，映射到主机的`REDIS_PORT`。
- 启动Redis服务器时设置密码和开启AOF持久化。
- 将容器连接到`gvb-network`网络，并分配IP地址由`REDIS_HOST`环境变量指定。



### gvb-mysql

```
  gvb-mysql:
    build: ../build/mysql
    container_name: gvb-mysql
    restart: always
    volumes:
      - ${DATA_DIRECTORY}/data/mysql:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - TZ=Asia/Shanghai
    ports:
      - ${MYSQL_PORT}:3306
    networks:
      gvb-network:
        ipv4_address: ${MYSQL_HOST}

```

- 使用自定义的MySQL镜像构建命令`../build/mysql`。
- 容器名称为`gvb-mysql`。
- 设置容器在退出时自动重启。
- 将主机上的数据目录挂载到容器的`/var/lib/mysql`目录。
- 设置MySQL的root密码和时区。
- 暴露MySQL的默认端口3306，映射到主机的`MYSQL_PORT`。
- 将容器连接到`gvb-network`网络，并分配IP地址由`MYSQL_HOST`环境变量指定。



### gvb-server

```
  gvb-server:
    build: ../../gin-blog-server
    container_name: gvb-server
    restart: always
    depends_on:
      gvb-redis:
        condition: service_started
      gvb-mysql:
        condition: service_started
    volumes:
      - ${DATA_DIRECTORY}/file/uploaded:/gvb/public/uploaded
    environment:
      - TZ=Asia/Shanghai
      - SERVER_PORT=:${SERVER_PORT}
      - MYSQL_HOST=${MYSQL_HOST}
      - MYSQL_PORT=3306
      - MYSQL_DBNAME=gvb
      - MYSQL_USERNAME=root
      - MYSQL_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - REDIS_ADDR=${REDIS_HOST}:6379
      - REDIS_PASSWORD=${REDIS_PASSWORD}
    ports:
      - ${SERVER_PORT}:${SERVER_PORT}
    networks:
      gvb-network:
        ipv4_address: ${BACKEND_HOST}

```

- 使用自定义的Gin Blog Server镜像构建命令`../../gin-blog-server`。
- 容器名称为`gvb-server`。
- 设置容器在退出时自动重启。
- 设置服务依赖`gvb-redis`和`gvb-mysql`，只有在它们启动后才会启动。
- 将主机上的上传文件目录挂载到容器的`/gvb/public/uploaded`目录。
- 设置环境变量，包括时区、服务器端口、MySQL和Redis的连接信息。
- 暴露服务端口，映射到主机的`SERVER_PORT`。
- 将容器连接到`gvb-network`网络，并分配IP地址由`BACKEND_HOST`环境变量指定。





### gvb-web

```
  gvb-web:
    build: ${WEB_BUILD_CONTEXT}
    container_name: gvb-web
    restart: always
    volumes:
      - ./server.crt:/etc/nginx/crt/server.crt
      - ./server.key:/etc/nginx/crt/server.key
    environment:
      - USE_HTTPS=${USE_HTTPS}
      - SERVER_NAME=${SERVER_NAME}
      - BACKEND_HOST=${BACKEND_HOST}
      - SERVER_PORT=${SERVER_PORT}
    ports:
      - "80:80"
      - "443:443"
    networks:
      gvb-network:
        ipv4_address: ${FRONTEND_HOST}

```

- 使用环境变量`${WEB_BUILD_CONTEXT}`指定的Dockerfile构建Web容器。
- 容器名称为`gvb-web`。
- 设置容器在退出时自动重启。
- 将主机上的证书和密钥文件挂载到容器的Nginx证书目录。
- 设置环境变量，包括是否使用HTTPS、服务器名称、后端服务地址和端口。
- 暴露HTTP和HTTPS端口，分别映射到主机的80和443端口。
- 将容器连接到`gvb-network`网络，并分配IP地址由`FRONTEND_HOST`环境变量指定。

## 自定义构建命令

在build选择中，填写含有dockerfile文件的目录，用于自定义构建



## .env

docker-compose.yml 同目录下的 .env 文件会被加载为其环境变量

```
# https://docs.docker.com/compose/environment-variables/
# docker-compose.yml 同目录下的 .env 文件会被加载为其环境变量

WEB_BUILD_CONTEXT=../..

# 数据存储的文件夹位置 (默认在 start 目录下生成 gvb 文件夹)
DATA_DIRECTORY=./gvb

# Redis
REDIS_PORT=63799
REDIS_PASSWORD=66554321

# MySQL
MYSQL_PORT=33066
MYSQL_ROOT_PASSWORD=12345566

# 后端服务配置
SERVER_PORT = 8765 # 服务端口

# 前端服务配置
# 要开启 https 请在 start 目录添加有效的证书文件: server.crt 和 server.key
USE_HTTPS=false
SERVER_NAME=localhost # 域名 或 localhost

# Docker Network: 一般不需要变, 除非发生冲突
SUBNET=172.12.0.0/24
REDIS_HOST=172.12.0.2
MYSQL_HOST=172.12.0.3
BACKEND_HOST=172.12.0.4
FRONTEND_HOST=172.12.0.6
```




---
title: docker-compose
pubDate: 2024-09-218
categories: ['blog','docker']
description: ''
---
# docker-compose

## OverView

Docker Compose 是 Docker 生态系统中的一个重要工具，它简化了多容器应用程序的配置和管理过程。

Docker Compose 是一个用于定义和运行多容器的Docker应用程序的工具。它使用一个yaml文件来配置应用程序的服务，网络和卷，然后使用一个简单的命令行界面来启动和停止整个应用程序。



### 多容器编排

Docker Compose 允许你在一个文件中定义多个容器，每个容器可以运行不同的服务，如web服务器，数据库，缓存等



### 配置文件

使用YAML文件docker-compose.yml 文件来配置应用程序。这个文件定义了服务，网络，卷和其他配置



### 一键部署

通过docker-compose up 命令，可以启动定义在docker-compose.yml 文件中的所有服务。同样的 docker-compose down 可以停止并移除这些服务



### 服务隔离

每个服务在Docker Compose 中运行在隔离的环境中，有自己的网络和存储卷，这有助于保持应用程序的模块化和可移植性



### 环境变量和变量替换

可以在 `docker-compose.yml` 文件中定义环境变量，或者在命令行中通过 `-e` 或 `--env-file` 选项来设置环境变量。



### 依赖管理

Docker Compose 允许你定义服务之间的依赖关系，确保在启动应用程序时，依赖的服务会按正确的顺序启动。



### 扩展性

虽然 Docker Compose 主要用于开发和测试环境，但它也可以用于生产环境，通过使用多个 Docker Compose 文件和 `docker-compose -f` 选项来管理更复杂的部署。



### 版本控制

`docker-compose.yml` 文件可以被版本控制系统跟踪，这有助于团队协作和应用程序的持续集成/持续部署（CI/CD）流程。



## 常用命令

### 启动命令

```
docker-compose up
```



### 后台运行服务

```
docker-compose up -d
```



### 停止服务

要停止并删除容器，但不删除数据卷

```
docker-compose down
```





### 停止并删除服务，包括数据卷

如果你想要完全移除服务及其数据，可以使用

```
docker-compose down --volumes
```





### 构建服务

如果你的服务定义中包含了构建上下文（例如，使用 `build` 而不是 `image`），你可以使用以下命令构建服务

```
docker-compose build
```





### 运行一次性命令

在服务的容器中运行一次性命令

```
docker-compose run web bash
```





### 管理服务

可以使用 `docker-compose ps` 来列出所有服务的容器

```
docker-compose ps
```





### 扩展服务

Docker Compose 允许你扩展服务的副本数量

```
docker-compose up --scale web=3
```



### 使用环境变量

在 `docker-compose.yml` 文件中使用环境变量，或者通过命令行设置

```
docker-compose up -e "MY_ENV_VAR=value"
```





### 使用多个配置文件

可以使用多个 `docker-compose` 文件，并指定使用哪个文件

```
docker-compose -f docker-compose.yml -f overrides.yml up
```


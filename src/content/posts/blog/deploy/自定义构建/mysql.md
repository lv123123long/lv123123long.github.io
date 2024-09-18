---
title: 自定义构建-mysql
pubDate: 2024-09-17
categories: ['blog']
description: ''
---

# 自定义构建mysql

在构建过程中，Docker 会将指定的上下文路径下的所有文件发送到 Docker 守护进程，并根据 `Dockerfile` 进行镜像构建。因此，确保所有构建所需的文件都位于指定的上下文目录中



## OverView

这段Dockerfile的用途是创建一个包含自定义MySQL配置和初始化数据的Docker镜像。在镜像构建完成后，可以通过运行这个镜像来启动一个MySQL服务器，并自动执行初始化脚本，从而初始化数据库。

```
FROM mysql:8.0

# 定义工作目录
ENV WORK_PATH /usr/local/work
# 定义被容器自动执行的目录
ENV AUTO_RUN_DIR /docker-entrypoint-initdb.d
# 定义要执行的 shell 文件
ENV RUN_SHELL run.sh

COPY ./mysql.cnf /etc/mysql/conf.d/
# 把数据库初始化数据的文件复制到工作目录
COPY ./gvb.sql ${WORK_PATH}/
# 把执行初始化的脚本放到 /docker-entrypoint-initdb.d 目录下
COPY ./${RUN_SHELL} ${AUTO_RUN_DIR}/

# 给执行文件添加可执行权限
RUN chmod a+x ${AUTO_RUN_DIR}/${RUN_SHELL}
```

1. `FROM mysql:8.0`：这行代码指定了基础镜像为MySQL 8.0。Docker镜像构建时会基于这个镜像进行构建。
2. `ENV WORK_PATH /usr/local/work`：这行代码定义了一个环境变量`WORK_PATH`，其值为`/usr/local/work`。这个变量可以在后续的Dockerfile中使用。
3. `ENV AUTO_RUN_DIR /docker-entrypoint-initdb.d`：这行代码定义了另一个环境变量`AUTO_RUN_DIR`，其值为`/docker-entrypoint-initdb.d`。这个目录是MySQL容器在初始化数据库时自动执行的脚本目录。
4. `ENV RUN_SHELL run.sh`：这行代码定义了第三个环境变量`RUN_SHELL`，其值为`run.sh`。这个变量表示要执行的shell脚本文件名。
5. `COPY ./mysql.cnf /etc/mysql/conf.d/`：这行代码将当前目录下的`mysql.cnf`文件复制到镜像中的`/etc/mysql/conf.d/`目录下。`mysql.cnf`文件通常用于配置MySQL服务器的参数。
6. `COPY ./gvb.sql ${WORK_PATH}/`：这行代码将当前目录下的`gvb.sql`文件复制到镜像中的`${WORK_PATH}/`目录下。`${WORK_PATH}`是之前定义的环境变量，其值为`/usr/local/work`。
7. `COPY ./${RUN_SHELL} ${AUTO_RUN_DIR}/`：这行代码将当前目录下的`${RUN_SHELL}`文件复制到镜像中的`${AUTO_RUN_DIR}/`目录下。`${RUN_SHELL}`是之前定义的环境变量，其值为`run.sh`。
8. `RUN chmod a+x ${AUTO_RUN_DIR}/${RUN_SHELL}`：这行代码给`${AUTO_RUN_DIR}/${RUN_SHELL}`文件添加可执行权限。`${AUTO_RUN_DIR}`是之前定义的环境变量，其值为`/docker-entrypoint-initdb.d`，`${RUN_SHELL}`是之前定义的环境变量，其值为`run.sh`。

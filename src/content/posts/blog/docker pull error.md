---
title: docker pull error
pubDate: 2024-09-17
categories: ['blog', 'docker']
description: ''
---

# docker pull error

报错 ：Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)

解决方法
配置加速地址

```
vim /etc/docker/daemon.json
```



```
{
  "registry-mirrors": ["https://docker.1panel.live"]
}
```



重启docker

```
systemctl restart  docker
```





## 参考

[docker pull 报错Get “https://registry-1.docker.io/v2/“: net/http: request canceled while waiting for c-CSDN博客](https://blog.csdn.net/jhgj56/article/details/142209517)


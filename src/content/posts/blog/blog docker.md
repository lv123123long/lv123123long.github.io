---
title: ubuntu docker
pubDate: 2024-09-17
categories: ['blog', 'docker']
description: ''
---

# ubuntu docker

## ubuntu

开始安装
阿里云Docker CE镜像站地址:

```
https://developer.aliyun.com/mirror/docker-ce?spm=a2c6h.13651102.0.0.57e31b11Cs9fUU

```



1. 卸载掉旧版:
  ```
  for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt remove $pkg; done
  
  ```

  

2. 安装GPG证书: 最新的方式是创建/etc/apt/keyrings文件夹，然后添加到这个文件夹下面。
  ```
  sudo apt update
  
  ```

  ```
  sudo apt install ca-certificates curl gnupg
  
  ```

  ```
  sudo install -m 0755 -d /etc/apt/keyrings
  
  ```

  ```
  mkdir /etc/apt/keyrings
  
  ```

  ```
  curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
  
  ```

  ```
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
  ```

  ```
  sudo chmod a+r /etc/apt/keyrings/docker.gpg
  
  ```

  

3. 安装docker-ce
  ```
  sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
  
  ```

  



## 参考

[Ubuntu通过阿里云镜像站安装docker，安装GPG证书的新方式_ubuntu gpg-CSDN博客](https://blog.csdn.net/weixin_44430301/article/details/129561939)
# V2fly_two


# 开始使用

## 注意点

* V2Ray 对于时间有比较严格的要求，要求服务器和客户端时间差绝对值不能超过**90 秒**，所以一定要保证时间足够准确。

## 安装

* 软件上 V2Ray 不区分服务器版和客户端版，也就是说在服务器和客户端运行的 V2Ray 是同一个软件，区别只是配置文件的不同。

### Centos

```
$ yum makecache
$ yum install curl
```

* 先安装curl

```
curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh -v -k
```

* 下载安装脚本
* -O 指定下载URl   -k：跳过证书验证  -v：打印详细debug

```
*   Trying ::...
* Connection refused
* Failed connect to raw.githubusercontent.com:443; Connection refused
* Closing connection 0
curl: (7) Failed connect to raw.githubusercontent.com:443; Connection refused
```

遇到的报错，怎么解决？

关于出现上面这个报错，是因为GitHub的`raw.githubusercontent.com`[域名解析](https://cloud.tencent.com/product/cns?from_column=20065&from=20065)被污染了。

1.查询真实IP

ping一下域名，或者

通过[https://www.ipaddress.com/](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Flinks.jianshu.com%2Fgo%3Fto%3Dhttps%3A%2F%2Fwww.ipaddress.com%2F&source=article&objectId=1684772)查询raw.githubusercontent.com的真实IP。

2.写入到/etc/hosts 中，重新执行下载命令即可

3.后面发现curl下载是空的，改为wget下载

```
wget https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh --no-check-certificate
```



### 最终解决办法

* 让机器使用科学上网，网关配置为：openwrt地址

```
bash install-release.sh
```

安装好之后，使用systemctl命令进行启动



# DNS


# DNS服务

## Overview

V2Ray 内置的 DNS 服务，其解析的 IP 往往先是用来匹配路由规则，如果配置不当，请求会在 DNS 请求上耗费一定时间。

路由 routing 的`"domainStrategy"`的几种模式都跟 DNS 功能密切相关

- `"AsIs"`，当终端请求一个域名时，进行路由里面的 domain 进行匹配，不管是否能匹配，直接按路由规则走。
- `"IPIfNonMatch"`, 当终端请求一个域名时，进行路由里面的 domain 进行匹配，若无法匹配到结果，则对这个域名进行 DNS 查询，用结果的 IP 地址再重新进行 IP 路由匹配。
- `"IPOnDemand"`, 当匹配时碰到任何基于 IP 的规则，将域名立即解析为 IP 进行匹配。

可见，`AsIs`是最快的，但是分路由的结果最不精确；而`IPOnDemand`是最精确的，但是速度是最慢的。

## DNS 流程

![image-20231224175455231](assets/image-20231224175455231.png)



## 基本配置

```
{
 "dns": {
   "servers": [
     "1.1.1.1",
     "localhost"
   ]
 }
}
```

DNS 模块的基础使用并没有什么特别复杂的地方，就是指定一个或几个 DNS 服务器，v2ray 会依次使用（查询失败时候会查询下一个）。其中"localhost"的意义是特殊的，作用是本地程序用系统的 DNS 去发请求，而不是 V2ray 直接跟 DNS 服务器通信，这个通信不受 Routing 等模块的控制。

## 客户端分流

* DNS服务是可以用来分流的
* 哪些域名要去哪个DNS服务器解析，并希望得到属于那里的IP，
* 如果得到的地址并不是国内的，则进行下一个 DNS 服务器进行查询，并使用新的结果。
* 


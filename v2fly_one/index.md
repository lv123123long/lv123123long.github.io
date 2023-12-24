# V2fly_one


# V2Fly

## OverView

```
A platform for building proxies to bypass network restrictions
```

* proxies：代理
* platform：平台
* bypass：绕过
* restrictions：限制
* 一个用于构建代理以绕过网络限制的平台。



## Related Links

* documentation：文档
* newcomer's Instructions ：新人须知

## Newcomer's Instructions

### 服务器

你需要一台防火墙外的服务器，来运行服务器端的 V2Ray

```
{
    "inbounds": [
        {
            "port": 10086, // 服务器监听端口
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": "b831381d-6324-4d53-ad4f-8cda48b30811"
                    }
                ]
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom"
        }
    ]
}
```

* 服务器中确保id和端口，与客户端一致，就可以正常连接了



### 客户端

在你的 PC（或手机）中，需要用以下配置运行 V2Ray ：

```

    ],
    "outbounds": [
        {
            "protocol": "vmess",
            "settings": {
                "vnext": [
                    {
                        "address": "server", // 服务器地址，请修改为你自己的服务器 ip 或域名
                        "port": 10086, // 服务器端口
                        "users": [
                            {
                                "id": "b831381d-6324-4d53-ad4f-8cda48b30811"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "protocol": "freedom",
            "tag": "direct"
        }
    ],
    "routing": {
        "domainStrategy": "IPOnDemand",
        "rules": [
            {
                "type": "field",
                "ip": [
                    "geoip:private"
                ],
                "outboundTag": "direct"
            }
        ]
    }
}
```

* 上述配置唯一要更改的地方是你的服务器 IP，配置中已注明。上述配置会把除局域网（比如访问路由器）以外的所有流量转发至你的服务器。

### 运行

- 在 Windows 和 macOS 中，配置文件通常是 V2Ray 同目录下的 `config.json` 文件。直接运行 `v2ray` 或 `v2ray.exe` 即可。
- 在 Linux 中，配置文件通常位于 `/etc/v2ray/` 或 `/usr/local/etc/v2ray/` 目录下。运行 `v2ray run -c /etc/v2ray/config.json`，或使用 systemd 等工具将 V2Ray 作为服务在后台运行。

> 可以使用 `v2ray help` 查看具体命令。在 `5.*` 版本之前，运行 `v2ray --config=/etc/v2ray/config.json`.

更多详细的说明可以参考 [配置文档](https://www.v2fly.org/config/overview.html) 和 [新白话文指南open in new window](https://guide.v2fly.org/)。



## 区别

客户端：V2Ray只是一个内核，V2Ray 上的图形客户端大多是调用 V2Ray 内核套一个图形界面的外壳，类似于 Linux 内核和 Linux 操作系统的关系；而 Shadowsocks 的客户端都是自己重新实现了一遍 Shadowsocks 协议。本文的内容短期内不涉及图形客户端的使用。

分流：也许大家第一反应是 PAC，实际上无论是 Shadowsocks（特指 Shadowsocks-libev）还是 V2Ray 本身不支持 PAC，都是客户端加进来的；Shadowsocks 的分流使用 ACL，V2Ray 使用自己实现的路由功能，孰优孰劣只是仁者智者的问题。

分享链接和二维码：V2Ray 不像 Shadowsocks 那样有统一规定的 URL 格式，所以各个 V2Ray 图形客户端的分享链接/二维码不一定通用。

加密方式：V2Ray（特指 VMess 协议）不像 Shadowsocks 那样看重对加密方式的选择，并且 VMess 的加密方式是由客户端指定的，服务器自适应。

时间：使用 V2Ray 要保证时间准确，因为这是为了安全设计的。

密码：V2Ray（VMess）只有 id（使用 UUID 的格式），作用类似于 Shadowsocks 的密码，但随机性远好于 Shadowsocks 的密码，只是不太方便记忆（安全和方便的矛盾）。

UDP转发：VMess 是基于 TCP 的协议，对于 UDP 包 V2Ray 会转成 TCP 再传输的，即 UDP over TCP。要 UDP 转发功能在客户端的 socks 协议中开启 UDP 即可。

路由器翻墙：实际上它们并没有什么区别，不要以为没有插件就不能在路由器上用，看事物请看本质。



# Vmess


# VMess

## OverView

VMess协议是由V2ray原创并使用于V2ray的加密传输协议，如同Shadowsocks一样为了对抗墙的深度包检测而研发的

## 配置文件说明

V2ray使用inbound（传入）和outbound（传出）的结构

形象地说，我们可以把 V2Ray 当作一个盒子，这个盒子有入口和出口(即 inbound 和 outbound)，我们将数据包通过某个入口放进这个盒子里

* 这个盒子以某种机制（这个机制解释路由）决定数据包从哪个出口出来

### 客户端

V2ray做客户端，则inbound接受来自浏览器的数据，由outbound发出去（通常是到V2ray服务器）

```
{
  "inbounds": [
    {
      "port": 1080, // 监听端口
      "protocol": "socks", // 入口协议为 SOCKS 5
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      },
      "settings": {
        "auth": "noauth"  //socks的认证设置，noauth 代表不认证，由于 socks 通常在客户端使用，所以这里不认证
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess", // 出口协议
      "settings": {
        "vnext": [
          {
            "address": "serveraddr.com", // 服务器地址，请修改为你自己的服务器 IP 或域名
            "port": 16823,  // 服务器端口
            "users": [
              {
                "id": "b831381d-6324-4d53-ad4f-8cda48b30811",  // 用户 ID，必须与服务器端配置相同
                "alterId": 0 // 此处的值也应当与服务器相同
              }
            ]
          }
        ]
      }
    }
  ]
}
```

在配置当中，有一个 id (在这里的例子是 b831381d-6324-4d53-ad4f-8cda48b30811)，作用类似于 Shadowsocks 的密码(password), VMess 的 id 的格式必须与 UUID 格式相同。关于 id 或者 UUID 没必要了解很多，在这里只要清楚以下几点就足够了：

- 相对应的 VMess 传入传出的 id 必须相同（如果你不是很明白这句话，那么可以简单理解成服务器与客户端的 id 必须相同）
- 由于 id 使用的是 UUID 的格式，我们可以使用任何 UUID 生成工具生成 UUID 作为这里的 id。比如 [UUID Generator (opens new window)](https://www.uuidgenerator.net/)这个网站，只要一打开或者刷新这个网页就可以得到一个 UUID，如下图。或者可以在 Linux 使用命令 `cat /proc/sys/kernel/random/uuid` 生成。

### 服务端

V2ray做服务端，则inbound是接受来自客户端的数据包，outbound是发出去的（通常是发送到想要访问的目标网站）

```
{
  "inbounds": [
    {
      "port": 16823, // 服务器监听端口
      "protocol": "vmess",    // 主传入协议
      "settings": {
        "clients": [
          {
            "id": "b831381d-6324-4d53-ad4f-8cda48b30811",  // 用户 ID，客户端与服务器必须相同
            "alterId": 0
          }
        ]
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",  // 主传出协议
      "settings": {}
    }
  ]
}
```



#### 检查是否配置成功

```
/usr/local/bin/v2ray test -config /usr/local/etc/v2ray/config.json
```



## 工作原理

### 客户端

客户端配置中的 inbounds，port 为 1080，即 V2Ray 监听了一个端口 1080，协议是 socks。之前我们已经把浏览器的代理设置好了（SOCKS Host: 127.0.0.1，Port: 1080）

假如访问了 google.com，浏览器就会发出一个数据包打包成 socks 协议发送到本机（127.0.0.1 指的本机，localhost）的 1080 端口，这个时候数据包就会被 V2Ray 接收到。

再看 outbounds，protocol 是 vmess，说明 V2Ray 接收到数据包之后要将数据包打包成 [VMess (opens new window)](https://www.v2fly.org/developer/protocols/vmess.html)协议并且使用预设的 id 加密（这个例子 id 是 b831381d-6324-4d53-ad4f-8cda48b30811），然后发往服务器地址为 serveraddr.com 的 16823 端口。服务器地址 address 可以是域名也可以是 IP，只要正确就可以了。

在客户端配置的 inbounds 中，有一个 `"sniffing"` 字段，V2Ray 手册解释为“流量探测，根据指定的流量类型，重置所请求的目标”，这话不太好理解，简单说这东西就是从网络流量中识别出域名。这个 sniffing 有两个用处：

1. 解决 DNS 污染；
2. 对于 IP 流量可以应用后文提到的域名路由规则；
3. 识别 BT 协议，根据自己的需要拦截或者直连 BT 流量



### 服务端

接着看服务器，服务器配置的 id 是 b831381d-6324-4d53-ad4f-8cda48b30811，所以 V2Ray 服务器接收到客户端发来的数据包时就会尝试用 b831381d-6324-4d53-ad4f-8cda48b30811 解密，如果解密成功再看一下时间对不对，对的话就把数据包发到 outbound 去，outbound.protocol 是 freedom（freedom 的中文意思是自由，在这里姑且将它理解成直连吧），数据包就直接发到 google.com 了。

实际上数据包的流向就是：

1. 浏览器
2. socks
3. v2ray客户端 inbound
4. v2ray客户端 outbound
5. VMess
6. V2ray服务端 inbound
7. V2ray服务端 outbound
8. Freedom
9. 目标网站

配置中还有一个 alterId 参数，在之前的版本中建议设置为 30 到 100 之间，在 v4.28.1 版本之后必须设置为 0 以启用 VMessAEAD 。

有人疑惑请求发出去后数据怎么回来，毕竟大多数的场景是下载。这个其实不算是问题，既然请求通过 V2Ray 发出去了，响应数据也会通过 V2Ray 原路返回（也许会有朋友看到这话会马上反驳说不一定是原路返回的，有这种想法的估计是非常了解 TCP/IP 协议的，何必较这个劲，这是底层的东西，又掌控在运营商手里，从应用层理解原路返回又有何不可）

## 注意事项

- 为了让浅显地介绍 V2Ray 的工作方式，本节中关于原理简析的描述有一些地方是错误的。但我知识水平又不够，还不知道该怎么改，暂且将错就错。正确的工作原理在用户手册的 [VMess 协议 (opens new window)](https://www.v2fly.org/developer/protocols/vmess.html)有详细的说明。
- id 为 UUID 格式，请使用软件生成，不要尝试自己造一个，否则很大程度上造出一个错误的格式来。
- VMess 协议可以设定加密方式，但 VMess 不同的加密方式对于过墙没有明显差别，本节没有给出相关配置方式（因为这不重要，默认情况下 VMess 会自己选择一种比较合适的加密方式），具体配置可见 [V2Ray 手册 (opens new window)](https://www.v2fly.org/developer/protocols/vmess.html)，不同加密方式的性能可参考[性能测试](https://guide.v2fly.org/app/benchmark.html)。



## 报错处理

### 客户端闪退

* 不能直接执行客户端的v2ray.exe程序
* 需要在cmd命令中配置参数

### 浏览器报错错误代码：PR_END_OF_FILE_ERROR

* 


# My_blog


# My blog

## hugo.toml

* `hugo serve` 的默认运行环境是 `development`, 而 `hugo` 的默认运行环境是 `production`.

  由于本地 `development` 环境的限制, **评论系统**, **CDN** 和 **fingerprint** 不会在 `development` 环境下启用.

  你可以使用 `hugo serve -e production` 命令来开启这些特性.

## CDN配置

CDN（Content Delivery Network）是一种通过在全球各地部署服务器节点来分发网站内容的网络服务。CDN的主要目的是提高网站的访问速度、减轻源服务器负载以及增加网站的可靠性。

CDN配置的作用是将网站的静态资源（如图片、CSS、JavaScript等）缓存到CDN服务器上，并通过CDN网络将这些资源分发给用户。具体的CDN配置包括以下几个方面：

1. 域名解析：将需要加速的域名（[如static.example.com](http://xn--static-hh4k.example.com/)）通过DNS解析指向CDN服务商提供的域名服务器。
2. 加速区域选择：根据网站受众的地理位置，选择合适的CDN服务器节点部署区域，以便更快地将内容传输给用户。
3. 缓存策略配置：根据网站的需求，设置CDN节点对静态资源的缓存时间、缓存容量、缓存更新机制等，以提供更好的用户体验和缓解源服务器压力。
4. URL重写：将网站的资源URL重写为CDN服务器的地址，使用户请求的资源能够经由CDN节点获取，而不是直接请求源服务器。
5. HTTPS支持：配置CDN证书，使CDN节点能够支持网站的HTTPS加密传输，确保数据的安全性。


# 20231224one


# V2ray代码解读

## OverView

### 依赖的第三方库

- In production:
  - [gorilla/websocket](https://github.com/gorilla/websocket)
  - [lucas-clemente/quic-go](https://github.com/lucas-clemente/quic-go)
  - [pires/go-proxyproto](https://github.com/pires/go-proxyproto)
  - [seiflotfy/cuckoofilter](https://github.com/seiflotfy/cuckoofilter)
  - [google/starlark-go](https://github.com/google/starlark-go)
  - [jhump/protoreflect](https://github.com/jhump/protoreflect)
  - [inetaf/netaddr](https://github.com/inetaf/netaddr)
- For testing only:
  - [miekg/dns](https://github.com/miekg/dns)
  - [h12w/socks](https://github.com/h12w/socks)

### 代码结构

#### 文件

.gitignore：就是在上传代码的时候，需要剔除哪些文件，不进行上传，不添加到仓库里面

annotations.go：定义了一个结构体，Annotation的结构体，用于文档注解功能，不会在其他地方使用

* 在注释中，以 "v2ray:" 开头的注解被视为是函数或类型的元数据。
* 该注解结构体包含一个 API 字段，用于指定注解的类型或函数是否可以在其他库中使用以及其稳定性。具体来说，API 字段可以具有以下可能的值：
* * v2ray:api:beta：表示该类型或函数已经准备好供使用，但未来可能会发生变化。
  * v2ray:api:stable：表示该类型或函数具有向后兼容的保证，稳定可靠。
  * v2ray:api:deprecated：表示该类型或函数已经过时，不应再使用。
* 如果没有标有 api 注解的类型或函数，则意味着它们不应该被外部使用。

##### config.go

字面意思了，表示这个项目的配置文件了

```
type ConfigLoader func(input interface{}) (*Config, error)
```

这段代码定义了一个类型为configLoader的函数签名

* 它描述了一个用于加载配置的函数类型
* `type ConfigLoader`：这里使用了 `type` 关键字来定义一个新的类型，名为 ConfigLoader。
* `func(input interface{}) (*Config, error)`：这是 ConfigLoader 类型所表示的函数签名。它定义了一个接受一个任意类型的参数 input，并返回一个指向 Config 类型对象以及一个 error 的函数。具体来说：
  - `(input interface{})`：这部分描述了函数的参数列表，其中 `input` 是一个类型为 `interface{}` 的参数。使用 `interface{}` 表示可以接受任意类型的参数。
  - `(*Config, error)`：这部分描述了函数的返回值，其中 `*Config` 表示返回一个指向 Config 类型对象的指针，`error` 表示返回一个错误对象。



##### v2ray.go

* 服务器是一个实例，任何时候，最多只能有一个server实例在运行
* 





#### 目录

##### main

* 就是程序入口的地方


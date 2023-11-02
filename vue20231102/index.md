# Vue20231102


# Vue20231102

## Vue通信方式

* props: 可以实现父子组件通信，props数据还是只读的！！！
* 需要在父组件中引用一下

```
import Child from './Child.vue'
```

## 事件

* 自定义事件
* * 可以实现组件之间的通信
* 原生DOM事件
* * event：事件对象
  * 带上多个参数

### 事件回调

script 

## 项目初始化

```
git clone https://gitee.com/jch1011/vue3_communication
```

```
npm i
```

安装好依赖后，运行一下

```
npm run dev
```

#### 报错

```
error when starting dev server:
Error: listen EACCES: permission denied 127.0.0.1:3000
```

* 端口被占用，换个端口启动

```
npm run dev -- --port 8080
```



---
title: ts学习
pubDate: 2024-09-09
categories: ['typescript']
description: ''
---

# Typescript

## Overview

* Typescript 是由微软开发的，是基于JavaScript的一个扩展语言
* TypeScript包含了javascript的所有内容，即typescript是javascript的超集
* TypeScript增加了静态类型检查，接口，泛型等很多现代开发特性
* typescript需要编译成javascript，然后交给浏览器或者javascript运行环境执行

## javascript缺点

1. 不清不楚的数据类型
2. 有漏洞的逻辑
3. 访问不存在的属性
4. 低级的拼写错误



## TypeScript  静态类型检查

在代码运行前进行检查，发现代码的错误或不合理之处，减少运行时异常的出现的几率，此种检查叫做**静态类型检查**



## 编译TypeScript

### 命令行编译

```
tsc ts文件
```

### 自动化编译

生成一个ts配置文件

```
tsc --init
```



监控所有ts文件

```
tsc --watch
```



#### tsconfig.json

有报错，不进行编译成js

```
onEmitOnError: true
```



### 框架脚手架

#### webpack



#### vite





## 类型声名

### 变量

```
let a: string
let b: number
let c: boolean
```



```
function count(x: number, y: number) :number {
	return x + y
}
```



## 类型总览

### JavaScript

1. string
2. number
3. boolean
4. null
5. underfined
6. bigint
7. symol
8. object (包含了 Array, Function, Date, Error等)



### Typescript

1. any
2. unknown
3. never
4. void
5. tuple
6. enum

### 自定义类型的方式

#### type



#### interface



### 注意点

在javascript中的这些内置构造函数：Number，String， Boolean 它们用于创建对应的包装对象，在日常开发中很少使用。

通常写小写的 number，string，boolean



1. 原始类型 VS 包装对象
   * 原始类型：占用的内存少，处理速度快
   * 包装对象：是复杂类型，在内存中占用的资源更多
2. 自动装箱：JavaScript 在必要时会自动将原始类型包装成对象，以便调用方法或访问属性





## 常用类型

### any

any：任意类型，放弃了类型检查

any类型的变量，可以赋值给容易的其他类型变量



### unknown

unknown：未知类型

unknown可以认为是一个安全的any，适用与

unknown不能赋值给其他类型变量，如果想赋值，需要以下方法

#### 判断语句



#### 断言



### never

never的含义是，任何值都不是

什么值都不能存

不是用来限制变量的，

可以用来限制函数的返回值



### void

void 通常用于函数返回值的声明：含义：【函数返回值为空，调用者也不能依赖其返回值进行任何操作】







* void是一个广泛的概念，用来表达“空”，而underfined 是空的一种

只能返回 undefined 



### object

实际开发中用的很少

object： 能存储的类型是【非原始类型】

Object：能存储的类型可以调用到 Object 方法的类型 (null  underfinal 不能存)





### 声明一个对象类型



### 声明函数类型



### 声明数组类型







### tuple

元组是一种特殊的数组类型  （不是关键字） 是一种形式

可以存储固定数量的元素，固定类型





### enum

枚举 

enum 可以定义`一组`命名`常量`



数字枚举，自动递增，方向映射

字符串枚举

常量枚举  const



### type

#### 联合类型



#### 交叉类型





## 类 class



## constructor 构造器





## 其他

创建html的模板

```
html:5 
```






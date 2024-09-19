---
title: main
pubDate: 2024-08-27
categories: ['blog']
description: ''
---

# main

## Overview

接受命令行参数

```
configPath := flag.String("c", "../config.yml", "配置文件路径")
flag.Parse()
```

1. **`configPath`**：这是一个变量名，类型为`*string`，即指向字符串的指针。这个变量将用于存储命令行标志的值。
2. **`flag.String`**：这是`flag`包中的一个函数，用于定义一个字符串类型的命令行标志。它接受四个参数：
   - 第一个参数是标志的名称，这里是`"c"`。
   - 第二个参数是标志的默认值，这里是`"../config.yml"`，表示如果用户没有在命令行中指定该标志，则使用这个默认值。
   - 第三个参数是标志的描述，当用户使用`-h`或`--help`选项时，这个描述会显示出来，这里是`"配置文件路径"`。
   - 第四个参数是一个指针，指向存储标志值的变量，这里是`configPath`。
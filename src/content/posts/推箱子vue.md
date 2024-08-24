---
title: vue推箱子初始化
pubDate: 2024-08-24
categories: ['VUE']
description: ''
---

# Vue推箱子初始化

```mermaid
graph LR
A[方行] --> B(圆角)
B --> C{条件a}
C --> |a=1| D[结果1]
C --> |a=2| E[结果2]
F[横向流程图]
```

## 流程设计图



```flow
st=>start: 开始框
op=>operation: 处理框
cond=>condition: 判断框(是或否?)
sub1=>subroutine: 子流程
io=>inputoutput: 输入输出框
e=>end: 结束框
st->op->cond
cond(yes)->io->e
cond(no)->sub1(right)->op
```






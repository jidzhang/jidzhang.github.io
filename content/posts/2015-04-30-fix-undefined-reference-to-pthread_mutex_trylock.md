---
title: "解决 undefined reference to 'pthread_mutex_trylock'"
date: 2015-04-30
draft: false
slug: "fix-undefined-reference-to-pthread_mutex_trylock"
categories:
  - "C/C++"
---

## 问题

编译时出现 `undefined reference to 'pthread_mutex_trylock'` 等 pthread 相关函数的链接错误。

## 原因

POSIX 线程库（pthread）默认不链接，需要手动指定。

## 解决

编译时添加 `-lpthread`：

```bash
gcc main.c -o test -lpthread
```

或在 Makefile 中：

```makefile
LDFLAGS = -lpthread
```

## 推荐

GCC 4.2+ 支持更规范的写法：

```bash
gcc main.c -o test -pthread
```

`-pthread` 会同时添加编译和链接所需的标志（包括预处理器宏），比 `-lpthread` 更完整。

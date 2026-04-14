---
title: "值得阅读源码的开源项目推荐"
date: 2015-03-11
draft: false
slug: "some-great-opensource-code"
categories:
  - "Code"
---

学习优秀的代码风格和架构设计，是提升编程能力的有效途径。以下是几个代码质量高、适合阅读学习的开源项目：

## C 语言项目

| 项目 | 特点 | 适合学习 |
|------|------|---------|
| **[nginx](https://github.com/nginx/nginx)** | 高性能 Web 服务器 | 事件驱动架构、内存池、模块化设计 |
| **[Linux Kernel](https://github.com/torvalds/linux)** | 操作系统内核 | 数据结构、并发控制、底层编程 |
| **[SQLite](https://www.sqlite.org/src)** | 嵌入式数据库 | SQL 解析、B 树实现、事务管理 |
| **[Redis](https://github.com/redis/redis)** | 内存数据库 | 数据结构、网络编程、单线程高性能 |
| **[mxml](https://github.com/michaelrsweet/mxml)** | 小型 XML 解析库 | 代码精简、接口设计 |
| **[lighttpd](https://github.com/lighttpd/lighttpd)** | 轻量 Web 服务器 | 网络编程、事件处理 |

## C++ 项目

| 项目 | 特点 | 适合学习 |
|------|------|---------|
| **[LLVM](https://github.com/llvm/llvm-project)** | 编译器基础设施 | 现代 C++、编译原理、架构设计 |
| **[Chromium](https://chromium.googlesource.com/chromium/src)** | 浏览器引擎 | 大型项目组织、多进程架构 |

## 阅读建议

- 从小项目开始（mxml、lighttpd），再读大项目
- 先理解整体架构，再深入具体模块
- 带着问题读代码：它怎么解决某个问题的？

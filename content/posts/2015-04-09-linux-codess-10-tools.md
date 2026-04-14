---
title: "C/C++ Linux 程序员必知的 10 类工具"
date: 2015-04-09
draft: false
slug: "linux-codess-10-tools"
categories:
  - "Linux"
---

## 1. 基本命令

Shell 操作是 Linux 开发的基础。掌握文件操作、管道、重定向、文本处理（grep/sed/awk）等。

## 2. 编辑器 — Vim / Emacs

Linux 下两大经典编辑器。至少精通一个，推荐 Vim（服务器环境几乎一定有）。

## 3. 构建工具 — Make / CMake

- **Make**：经典的构建自动化工具，通过 Makefile 定义编译规则
- **CMake**：跨平台构建系统，自动生成 Makefile 或 VS 项目文件，现代项目首选

## 4. 调试器 — GDB

命令行调试器，支持断点、单步、变量查看、内存检查。核心命令：

```bash
gdb ./program
(gdb) break main
(gdb) run
(gdb) next
(gdb) print variable
(gdb) backtrace
```

## 5. 版本控制 — Git

现代开发必备。SVN 和 CVS 已逐渐被 Git 取代。

```bash
git init / git clone
git add / git commit / git push
git branch / git merge
```

## 6. 代码导航 — ctags / cscope

在大型代码库中快速定位函数定义、调用关系。

```bash
ctags -R .              # 生成标签文件
cscope -Rbkq            # 建立索引
```

配合 Vim 使用，跳转代码效率极高。

## 7. 进程间通信（IPC）

Linux 提供多种 IPC 机制：管道、信号、消息队列、共享内存、信号量、Socket。理解这些是系统级编程的基础。

## 8. 多线程 — Pthreads / C++ Thread

- **POSIX Threads (pthreads)**：C 语言多线程标准接口
- **C++11 std::thread**：现代 C++ 内置线程库
- **Boost.Thread**：C++11 之前的多线程方案

## 9. 内存检测 — Valgrind

检测内存泄漏、越界访问、未初始化读取等问题：

```bash
valgrind --leak-check=full ./program
```

## 10. GUI 开发 — Qt

跨平台 C++ GUI 框架，支持信号/槽机制、网络、数据库、多线程等。Linux 下最成熟的 GUI 方案。

---

*参考：[OSChina](https://www.oschina.net/news/32307/10-things-c-c-linux-programmer-must-know)*

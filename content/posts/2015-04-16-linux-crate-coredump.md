---
title: "Linux 控制 Core Dump 核心转储文件"
date: 2015-04-16
draft: false
slug: "linux-crate-coredump"
categories:
  - "Linux"
---

## 什么是 Core Dump

程序崩溃时，系统可以将当时的内存映像写入磁盘文件（core dump），用于事后用 GDB 调试分析崩溃原因。

## 查看当前设置

```bash
# Bash
ulimit -c

# C Shell
limit coredumpsize
```

输出为 `0` 表示禁止生成，`unlimited` 表示无大小限制。

## 开启 Core Dump

```bash
# Bash（单位：KB）
ulimit -c unlimited    # 无限制
ulimit -c 102400       # 最大 100MB

# C Shell（单位：B）
limit coredumpsize unlimited
```

## 永久生效

在 `/etc/security/limits.conf` 中添加：

```
*  soft  core  unlimited
```

或编辑 `/etc/profile`，添加 `ulimit -c unlimited`。

## Core 文件位置

```bash
# 查看当前 core 文件路径模板
cat /proc/sys/kernel/core_pattern

# 设置路径（例如统一放到 /var/core/）
echo "/var/core/core.%e.%p.%t" | sudo tee /proc/sys/kernel/core_pattern
```

## 使用 Core Dump 调试

```bash
gdb ./program core
(gdb) bt       # 查看崩溃时的调用栈
```

---
title: "man 命令：在线手册页使用指南"
date: 2015-03-06
draft: false
slug: "howto-man-command"
categories:
  - "Linux"
---

## 基本用法

```bash
man [章节号] 命令名
```

## 章节说明

| 章节 | 内容 | 示例 |
|------|------|------|
| 1 | 用户命令 | `man 1 ls` |
| 2 | 系统调用 | `man 2 open` |
| 3 | C 库函数 | `man 3 printf` |
| 4 | 设备和特殊文件 | `man 4 null` |
| 5 | 配置文件格式 | `man 5 passwd` |
| 7 | 杂项（协议、字符集等） | `man 7 ascii` |
| 8 | 系统管理命令 | `man 8 mount` |

不指定章节号时，`man` 会按顺序搜索，返回第一个匹配结果。

## 常见陷阱

```bash
man printf    # 显示第 1 章：shell 命令 printf
man 3 printf  # 显示第 3 章：C 库函数 printf
```

两者完全不同。查 C 函数时务必指定章节号 `3`。

## 实用技巧

```bash
# 搜索手册页（不知道完整名称时）
man -k "copy files"    # 等同于 apropos

# 在 man 页面内搜索
# 按 / 输入关键词，n 下一个，N 上一个

# 查看手册页存储路径
man --path

# 以网页方式打开（部分系统支持）
man -H firefox open
```

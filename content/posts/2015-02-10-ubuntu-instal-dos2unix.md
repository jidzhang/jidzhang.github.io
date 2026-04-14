---
title: "Ubuntu 安装 dos2unix 文件格式转换工具"
date: 2015-02-10
draft: false
slug: "ubuntu-instal-dos2unix"
categories:
  - "Linux"
---

## 问题

Windows 和 Linux 的文本文件换行符不同（Windows 用 `\r\n`，Linux 用 `\n`），在 Linux 下打开 Windows 编辑的脚本会报错。需要用 `dos2unix` 转换。

Ubuntu 默认没有 `dos2unix`，但有替代方案。

## 方案一：安装 tofrodos（推荐）

```bash
sudo apt-get install tofrodos
```

安装后提供两个命令：

| 命令 | 作用 |
|------|------|
| `fromdos file` | Windows → Linux（相当于 dos2unix） |
| `todos file` | Linux → Windows（相当于 unix2dos） |

习惯原命令名的话，创建符号链接：

```bash
sudo ln -s /usr/bin/fromdos /usr/bin/dos2unix
sudo ln -s /usr/bin/todos /usr/bin/unix2dos
```

或在 `~/.bashrc` 中设置别名：

```bash
alias dos2unix=fromdos
alias unix2dos=todos
```

## 方案二：直接安装 dos2unix

较新版本的 Ubuntu 可以直接安装：

```bash
sudo apt-get install dos2unix
```

## 使用

```bash
dos2unix script.sh    # 转换为 Unix 格式
unix2dos file.txt     # 转换为 Windows 格式
```

---
title: "touch 命令：创建空文件或更新时间戳"
date: 2015-03-05
draft: false
slug: "howto-touch-command"
categories:
  - "Linux"
---

## 功能

`touch` 有两个用途：

1. **创建空文件** — 如果文件不存在，创建一个大小为 0 的空文件
2. **更新时间戳** — 如果文件已存在，更新其访问时间和修改时间为当前时间

## 常用示例

```bash
# 创建空文件
touch newfile.txt

# 同时创建多个文件
touch file1.txt file2.txt file3.txt

# 更新已有文件的时间戳
touch existing.txt

# 只更新访问时间（不修改 mtime）
touch -a file.txt

# 只更新修改时间（不修改 atime）
touch -m file.txt

# 设置为指定时间（格式：YYYYMMDDhhmm）
touch -t 202604011200 file.txt
```

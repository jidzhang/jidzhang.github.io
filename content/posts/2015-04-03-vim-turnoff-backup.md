---
title: "Vim 关闭自动备份文件"
date: 2015-04-03
draft: false
slug: "vim-turnoff-backup"
categories:
  - "Vim"
---

## 问题

Vim 默认在保存文件时会生成 `~` 后缀的备份文件（如 `config.yml~`），污染目录。

## 解决

在 Vim 中执行 `:e $MYVIMRC` 打开配置文件，添加：

```vim
set nobackup       " 关闭备份文件（file~）
set nowritebackup  " 关闭写入时的临时备份
set noswapfile     " 关闭交换文件（.file.swp）（可选）
```

如果只想对特定目录关闭备份：

```vim
set backupskip=/tmp/*,/private/tmp/*
```

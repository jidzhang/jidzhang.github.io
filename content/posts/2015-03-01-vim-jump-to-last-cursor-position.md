---
title: "Vim 打开文件时自动跳到上次的光标位置"
date: 2015-03-01
draft: false
slug: "vim-jump-to-last-cursor-position"
categories:
  - "Vim"
---

## 问题

在 Linux 下用 Vim 打开文件时，光标总是在第一行，而不是上次退出时的位置。Windows 下的 gVim 默认支持这个功能。

## 解决方法

将以下代码添加到 `~/.vimrc`：

```vim
if has("autocmd")
  filetype plugin indent on

  augroup vimrcEx
    au!
    autocmd BufReadPost *
      \ if line("'\"") > 1 && line("'\"") <= line("$") |
      \   exe "normal! g`\"" |
      \ endif
  augroup END
else
  set autoindent
endif
```

## 原理

- `line("'\"")` 获取上次退出时光标所在行号（Vim 自动在 `.viminfo` 中记录）
- `exe "normal! g`\""` 跳转到该位置（用 `g` 跳转，不改变 jumplist）
- 判断条件确保行号有效（大于 1 且不超过文件总行数）

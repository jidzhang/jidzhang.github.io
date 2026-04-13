---
title: "Vim自动跳到上次浏览文件时的位置"
date: 2015-03-01
draft: false
slug: "vim-jump-to-last-cursor-position"
categories:
  - "Vim"
---

在Windows下用gvim，这个选项是默认打开的。也就是用vim打开文件时，可以记忆退出时的位置，从而再次打开就会直接跳到上次的位置。

该项控制命令写在$VIMRUNTIME/vimrc_example.vim下，可以把下面的命令拷贝到 `{$HOME}/.vimrc` 文件中，本方法尤其适用于Linux用户：
```vim
" Only do this part when compiled with support for autocommands.
if has("autocmd")
```
filetype plugin indent on

" Put these in an autocmd group, so that we can delete them easily.
augroup vimrcEx
au!

autocmd BufReadPost *
    \ if line("'\"") > 1 && line("'\"") <= line("$") |
    \   exe "normal! g`\"" |
    \ endif

augroup END
```

else
```
set autoindent   " always set autoindenting on
```

endif " has("autocmd")
```

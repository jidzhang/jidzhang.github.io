---
title: "Linux 从源码编译安装 Vim"
date: 2013-11-02
draft: false
slug: "build-vim-on-linux"
categories:
  - "Geek"
---

## 为什么自己编译

系统包管理器安装的 Vim 通常没有开启 Python、Lua 等脚本支持，导致一些插件（如 YouCompleteMe、Neocomplete）无法使用。

## 编译步骤

### 1. 安装依赖

```bash
# Debian/Ubuntu
sudo apt-get install libncurses5-dev libgnome2-dev libgnomeui-dev \
    libgtk2.0-dev libatk1.0-dev libbonoboui2-dev libcairo2-dev \
    libx11-dev libxpm-dev libxt-dev python-dev ruby-dev lua5.2 \
    liblua5.2-dev libperl-dev git
```

### 2. 配置

```bash
./configure --with-features=huge \
    --enable-rubyinterp \
    --enable-luainterp=dynamic \
    --enable-pythoninterp \
    --with-python-config-dir=/usr/lib/python2.7/config \
    --enable-perlinterp \
    --enable-cscope \
    --enable-tclinterp \
    --enable-multibyte \
    --enable-clipboard \
    --prefix=/usr
```

**各选项说明：**

| 选项 | 作用 |
|------|------|
| `--with-features=huge` | 开启几乎所有功能 |
| `--enable-rubyinterp` | Ruby 脚本支持 |
| `--enable-luainterp=dynamic` | Lua 动态加载 |
| `--enable-pythoninterp` | Python 2 支持（YCM 需要） |
| `--enable-perlinterp` | Perl 脚本支持 |
| `--enable-cscope` | Cscope 代码浏览 |
| `--enable-multibyte` | 多字节字符（中文支持） |

> ⚠️ 不要同时开启 Python 2 和 Python 3，会导致 YouCompleteMe 无法正常工作。

### 3. 编译安装

```bash
make VIMRUNTIMEDIR=/usr/share/vim/vim74
sudo make install
```

### 4. 验证

```bash
vim --version | head -n 3
# 应显示编译选项中包含 +python、+lua 等
```

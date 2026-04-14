---
title: "Cygwin 安装完整 man-pages"
date: 2015-03-02
draft: false
slug: "cygwin-install-lastest-manpages"
categories:
  - "Linux"
---

## 问题

Cygwin 自带的 man pages 不完整，比如 `man listen`、`man socket` 等系统调用没有文档。

## 解决方法

### 方法一：编译安装（推荐）

```bash
# 1. 下载最新 man-pages
# https://www.kernel.org/pub/linux/docs/man-pages/

# 2. 解压
tar zxvf man-pages-3.41.tar.gz
cd man-pages-3.41

# 3. 安装
make gz
make install
```

### 方法二：手动放置

1. 下载并解压到 Cygwin 安装目录的 `%cygwin%\usr\man` 下
2. 编辑 `/etc/man.conf`，添加：

```
MANPATH /usr/man/man-pages-3.41
```

3. 重启 Cygwin 终端

> 版本号根据实际下载的版本修改。

## 验证

```bash
man listen
man socket
```

能正常显示说明安装成功。

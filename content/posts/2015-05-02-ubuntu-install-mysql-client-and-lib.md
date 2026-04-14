---
title: "Ubuntu 安装 MySQL 客户端及开发库"
date: 2015-05-02
draft: false
slug: "ubuntu-install-mysql-client-and-lib"
categories:
  - "Linux"
---

## 安装

### MySQL 服务器 + 客户端

```bash
sudo apt-get install mysql-server
```

安装 server 时会自动安装 client，无需单独安装。安装过程中会提示设置 root 密码。

### C/C++ 开发库

```bash
sudo apt-get install libmysqlclient-dev
```

安装后头文件在 `/usr/include/mysql/`，链接时加 `-lmysqlclient`。

## 验证

```bash
mysql --version
mysql -u root -p
```

## 编译示例

```bash
gcc -o myapp myapp.c $(mysql_config --cflags --libs)
```

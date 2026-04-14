---
title: "Ubuntu 一键安装/卸载 LAMP 环境"
date: 2015-05-10
draft: false
slug: "ubuntu-install-lamp"
categories:
  - "Linux"
---

> LAMP = Linux + Apache + MySQL + PHP

## 安装

```bash
sudo tasksel install lamp-server
```

安装过程中会提示设置 MySQL root 密码。

## 卸载

```bash
sudo tasksel remove lamp-server
```

> ⚠️ 卸载后务必更新系统，防止误删系统组件：

```bash
sudo apt-get update
sudo apt-get upgrade
```

## 验证

```bash
# 检查 Apache
apache2 -v
curl http://localhost

# 检查 MySQL
mysql --version

# 检查 PHP
php -v
```

## 单独安装各组件

```bash
sudo apt-get install apache2
sudo apt-get install mysql-server
sudo apt-get install php libapache2-mod-php php-mysql
```

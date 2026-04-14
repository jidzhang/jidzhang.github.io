---
title: "openSUSE 安装 MySQL"
date: 2015-04-27
draft: false
slug: "opensuse-install-mysql"
categories:
  - "Linux"
---

## 安装

```bash
sudo zypper install mysql-community-server
```

## 启动并设置开机自启

```bash
sudo systemctl enable mysql
sudo systemctl start mysql
```

## 安全初始化

```bash
sudo mysql_secure_installation
```

按提示设置 root 密码、移除匿名用户、禁止远程 root 登录等。

## 验证

```bash
mysql -u root -p
mysql --version
```

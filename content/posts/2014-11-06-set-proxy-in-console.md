---
title: "在Linux命令行中设置代理服务器"
date: 2014-11-06
draft: false
slug: "set-proxy-in-console"
categories:
  - "Linux"
---

### 前言
在使用apt-get或git pull或wget的时候经常因为国内网络限制的原因而考虑使用代理服务器，
这个时候就需要在命令行中设置代理，同时又不影响系统的代理设置。

### 方法
可以通过三种方法设置代理服务器

* 方法一

在终端中直接运行命令
```bash
export http_proxy=http://proxyAddress:port
```

这个办法的好处是简单直接，并且影响面很小（只对当前终端有效）。

* 方法二

把代理服务器地址写入shell配置文件
```bash
vi ~/.bashrc
```

文件末尾添加如下内容
```bash
http_proxy=http://proxyAddress:port
export http_proxy
```

然后ESC后:wq保存文件，接着在终端中执行
```bash
source ~/.bashrc
```

或者退出当前终端再起一个终端。
这个办法的好处是把代理服务器永久保存了，下次就可以直接用了。

* 方法三

改相应工具的配置，比如apt的配置
```bash
sudo vi /etc/apt/apt.conf
```

在文件末尾加入下面这行
```bash
Acquire::http::Proxy "http://proxyAddress:port"
```

保存apt.conf文件即可。

### 补充
如果代理服务器需要登陆，这时可以直接把用户名和密码写进去
```bash
http_proxy=http://userName:password@proxyAddress:port
```

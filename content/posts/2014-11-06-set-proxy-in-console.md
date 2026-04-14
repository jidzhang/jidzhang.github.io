---
title: "Linux 命令行设置代理服务器"
date: 2014-11-06
draft: false
slug: "set-proxy-in-console"
categories:
  - "Linux"
---

## 三种方式

### 1. 临时生效（仅当前终端）

```bash
export http_proxy=http://proxyAddress:port
export https_proxy=http://proxyAddress:port
```

关闭终端后失效，不影响系统设置。适合偶尔使用。

### 2. 永久生效（写入 Shell 配置）

在 `~/.bashrc` 末尾添加：

```bash
http_proxy=http://proxyAddress:port
https_proxy=http://proxyAddress:port
export http_proxy https_proxy
```

然后执行 `source ~/.bashrc` 生效。

### 3. 仅对特定工具生效

**apt：**
```bash
sudo vi /etc/apt/apt.conf
# 添加：
Acquire::http::Proxy "http://proxyAddress:port";
Acquire::https::Proxy "http://proxyAddress:port";
```

**git：**
```bash
git config --global http.proxy http://proxyAddress:port
```

**wget：**
```bash
# 编辑 ~/.wgetrc
use_proxy = on
http_proxy = http://proxyAddress:port
https_proxy = http://proxyAddress:port
```

## 带认证的代理

如果代理需要用户名和密码：

```bash
export http_proxy=http://userName:password@proxyAddress:port
export https_proxy=http://userName:password@proxyAddress:port
```

## 取消代理

```bash
unset http_proxy https_proxy
```

---
title: "停止和删除 Hasplms（Sentinel HASP）服务"
date: 2014-04-16
draft: false
slug: "delete-hasplms-service"
categories:
  - "Windows"
---

## 问题

使用圣天诺 HASP 加密的软件卸载后，`Hasplms`（Sentinel LDK License Manager）服务仍在后台运行，占用资源且开机自启。

## 解决方法

### 方法一：命令行删除

```cmd
# 1. 停止服务
sc stop hasplms

# 2. 禁用自启
sc config hasplms start=disabled

# 3. 删除服务
sc delete hasplms
```

> 需要管理员权限运行 cmd。

### 方法二：Sentinel 官方工具

1. 从 [Sentinel 官网](https://supportportal.gemalto.com/csm?id=kb_article_view&sys_kb_id=2f8a3566db84a110c4b5b895ca96192b) 下载 Command Line Run-time Installer（`haspdinst.exe`）
2. 执行卸载：

```cmd
haspdinst -fr -purge
```

如需重新安装：

```cmd
haspdinst -i -fi -kp
```

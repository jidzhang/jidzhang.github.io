---
title: "showmount : 查看NFS共享目录"
date: 2015-01-09
draft: false
slug: "howto-showmount"
categories:
  - "Linux"
---

> showmount -- show mount information for an NFS server

showmount常用法:
```bash
showmount              # 没有参数，列出所有挂载了共享目录的客户端client
showmount -a           # 列出server上共享的目录，同时列出client上的挂载点
showmount -d           # 列出被client挂载的目录
showmount -e           # 列出server端的共享目录
```

通常可以这样查询某个Server：

查询共享目录：`showmount –e serverIP(or serverName)`

查询被哪些客户端挂载：`showmount –a `

查看所有： `showmount`

---
title: "showmount：查看 NFS 共享目录"
date: 2015-01-09
draft: false
slug: "howto-showmount"
categories:
  - "Linux"
---

## 命令格式

```bash
showmount [选项] [ServerIP]
```

## 常用选项

| 选项 | 作用 |
|------|------|
| 无参数 | 列出所有挂载了共享目录的客户端 |
| `-a` | 列出服务端共享目录 + 客户端挂载点 |
| `-d` | 列出当前被客户端挂载的目录 |
| `-e` | 列出服务端导出的所有共享目录 |

## 使用示例

```bash
# 查看某台服务器的共享目录
showmount -e 192.168.1.100

# 查看谁在挂载
showmount -a 192.168.1.100

# 查看所有信息
showmount 192.168.1.100
```

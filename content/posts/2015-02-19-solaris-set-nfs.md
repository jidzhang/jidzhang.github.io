---
title: "Solaris 配置 NFS 共享"
date: 2015-02-19
draft: false
slug: "solaris-set-nfs"
categories:
  - "Solaris"
---

> 以下操作均需 root 权限。

## 服务端配置

### 1. 启动 NFS 服务

```bash
# Solaris 10+
svcadm enable network/nfs/server

# 旧版本
/etc/init.d/nfs.server start
```

### 2. 共享目录

**即时生效（重启后失效）：**

```bash
# 只读共享
share -F nfs -d "shared dir" /export/home/shared

# 读写共享，限定客户端
share -F nfs -o rw=192.168.1.100 -d "home dirs" /export/home2
```

**永久生效：**

将 share 命令写入 `/etc/dfs/dfstab`：

```bash
share -F nfs -d "shared dir" /export/home/shared
```

### 3. 验证共享

```bash
dfshares
# 或
showmount -e
```

---

## 客户端配置

### 1. 启动 NFS 客户端服务

```bash
svcadm enable network/nfs/client
```

### 2. 挂载共享目录

**临时挂载：**

```bash
mount -F nfs 192.168.1.1:/export/home/shared /mnt/shared
```

**永久挂载（写入 /etc/vfstab）：**

```
192.168.1.1:/export/home/shared - /mnt/shared nfs - yes -
```

> 注意：各字段之间用空格分隔，短横线 ` - ` 两侧都有空格。

### 3. 查看挂载状态

```bash
showmount -e 192.168.1.1    # 查看服务端共享的目录
showmount -a                # 查看所有客户端挂载情况
mount | grep nfs            # 查看本机 NFS 挂载
```

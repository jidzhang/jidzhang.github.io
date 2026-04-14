---
title: "Solaris 开启 FTP 和 SSH 服务"
date: 2015-01-05
draft: false
slug: "solaris-enable-ftp-and-ssh"
categories:
  - "Solaris"
---

## FTP 服务

### 启动 FTP

```bash
svcadm enable /network/ftp
```

### 查看状态

```bash
svcs -l network/ftp
```

### 允许 root 登录（不推荐）

编辑 `/etc/ftpd/ftpusers`，注释掉 root 那行。出于安全考虑，建议只给普通用户开放 FTP。

---

## SSH 服务

SSH 默认已启动。如果需要允许 root 登录：

### 修改配置

```bash
vi /etc/ssh/sshd_config
# 将 PermitRootLogin 改为 yes
```

### 重启 SSH

```bash
svcadm restart network/ssh
```

如果重启不生效：

```bash
svcadm refresh ssh
svcadm enable ssh
```

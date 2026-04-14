---
title: "FreeBSD 开启 SSH 远程登录"
date: 2015-04-18
draft: false
slug: "freebsd-enable-ssh-login"
categories:
  - "FreeBSD"
---

## 步骤

### 1. 启用 SSH 服务

编辑 `/etc/rc.conf`，确保有：

```bash
sshd_enable="yes"
```

编辑 `/etc/inetd.conf`，去掉 ssh 行前的 `#`。

### 2. 配置 SSHD

编辑 `/etc/ssh/sshd_config`，确保以下配置：

```bash
PermitRootLogin yes           # 允许 root 登录（按需开启）
PermitEmptyPasswords no       # 禁止空密码
PasswordAuthentication yes    # 允许密码认证
```

### 3. 重启服务

```bash
/etc/rc.d/sshd restart
```

### 4. 验证

```bash
# 本地测试
ssh localhost

# 远程连接
ssh user@服务器IP
```

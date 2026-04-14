---
title: "Solaris 用户主目录管理"
date: 2015-02-08
draft: false
slug: "home-directories-under-solaris"
categories:
  - "Solaris"
---

## 背景

Solaris 中用户主目录有两个位置：

- **`/home`** — 由 automounter（自动挂载器）控制，管理员不能直接在此创建目录
- **`/export/home`** — 管理员可以在此创建用户主目录

## 方式一：本地主目录（不使用 automounter）

```bash
# 创建用户
useradd -u 400 -g user -c "user oops" -m -d /export/home/oops oops

# 设置密码
passwd oops

# 设置权限
chown oops /export/home/oops
chgrp user /export/home/oops
```

`-m` 参数会自动创建主目录。

## 方式二：自动挂载主目录（NFS）

```bash
# 创建用户（不指定 -d，默认使用 /home/oops）
useradd -u 400 -g user -c "user oops" oops

# 设置密码
passwd oops

# 手动创建实际目录
mkdir /export/home/oops
chown oops /export/home/oops
chgrp user /export/home/oops

# 配置 automounter
vi /etc/auto_home
# 添加一行：
# oops  remotehost:/home/&
```

`remotehost:/home/&` 表示用户 oops 的主目录从远程主机挂载，`&` 是用户名的通配符。

---

*参考：[Unix Workstation Administration Tasks](http://ibgwww.colorado.edu/~lessem/psyc5112/usail/tasks/toc.html)*

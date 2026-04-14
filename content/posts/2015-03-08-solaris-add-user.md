---
title: "Solaris 创建用户"
date: 2015-03-08
draft: false
slug: "solaris-add-user"
categories:
  - "Solaris"
---

## 步骤

```bash
# 1. 创建主目录
mkdir -p /export/home/username

# 2. 创建用户（指定 Shell 和主目录）
useradd -s /usr/bin/bash -d /export/home/username username

# 3. 设置密码
passwd username

# 4. 设置主目录权限
chown username /export/home/username

# 5. 验证
tail -1 /etc/passwd
```

## 常用 useradd 选项

| 选项 | 作用 |
|------|------|
| `-s` | 指定登录 Shell |
| `-d` | 指定主目录 |
| `-g` | 指定主组 |
| `-G` | 指定附加组 |
| `-u` | 指定 UID |
| `-m` | 自动创建主目录（部分版本支持） |

## 删除用户

```bash
userdel username          # 保留主目录
userdel -r username       # 同时删除主目录
```

---

*参考：[Solaris System Administration Guide](https://docs.oracle.com/cd/E19253-01/817-1985/)*

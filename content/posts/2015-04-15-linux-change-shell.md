---
title: "Linux 更改用户登录 Shell"
date: 2015-04-15
draft: false
slug: "linux-change-shell"
categories:
  - "Linux"
---

## 方法

```bash
# 1. 确认目标 Shell 已安装
which zsh
# /usr/bin/zsh

# 2. 更改用户的默认 Shell
chsh -s /usr/bin/zsh yourname

# 3. 重新登录生效
```

## 查看可用的 Shell

```bash
cat /etc/shells
```

## 注意事项

- 必须使用 `/etc/shells` 中列出的 Shell 路径
- 普通用户只能改自己的 Shell，root 可以改任何人的
- 修改后下次登录生效，当前会话不变

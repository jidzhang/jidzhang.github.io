---
title: "Git 记住密码：避免每次 pull/push 都输入凭据"
date: 2013-10-15
draft: false
slug: "git-password-caching"
categories:
  - "Geek"
---

## 问题

每次 `git pull` 或 `git push` 都要输入用户名和密码，非常繁琐。

## 解决方法

### Linux / macOS（凭据缓存）

```bash
# 缓存密码 1 小时（3600 秒）
git config --global credential.helper 'cache --timeout=3600'

# 或缓存 8 小时
git config --global credential.helper 'cache --timeout=28800'
```

> 要求 Git 版本 ≥ 1.7.10。

### Windows（凭据管理器）

安装 [Git Credential Manager for Windows](https://github.com/microsoft/Git-Credential-Manager-for-Windows)，安装后 Git 会自动使用 Windows 凭据管理器存储密码。

新版 Git for Windows 已内置此功能，无需额外安装。

### 永久存储（所有平台）

```bash
# 明文存储在 ~/.git-credentials（不推荐在共享电脑上使用）
git config --global credential.helper store
```

## 安全提示

- `cache` 模式：密码仅存在内存中，超时后自动清除，相对安全
- `store` 模式：密码以明文保存在磁盘上，方便但不安全
- 推荐使用 SSH key 或个人访问令牌（PAT）替代密码认证

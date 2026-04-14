---
title: "Debian 开启 bash_completion 自动补全"
date: 2013-10-15
draft: false
slug: "enable-bash_completion-debian"
categories:
  - "Linux"
---

## 问题

Debian 默认的 bash 自动补全功能不完整——按 Tab 只能补全文件名和命令名，无法补全 `apt-get` 的子命令、`systemctl` 的服务名等。

## 原因

bash_completion 包其实已经预装了，只是默认被注释掉了。

## 解决方法

### 方法一：全局生效（需要 root）

编辑 `/etc/bash.bashrc`，找到被注释掉的这段代码：

```bash
# if ! shopt -oq posix; then
#   if [ -f /usr/share/bash-completion/bash_completion ]; then
#      . /usr/share/bash-completion/bash_completion
#   elif [ -f /etc/bash_completion ]; then
#      . /etc/bash_completion
#   fi
# fi
```

去掉每行前面的 `#`，保存，重新登录即可。

### 方法二：仅当前用户

把上面取消注释后的代码段追加到 `~/.bashrc` 末尾，然后执行：

```bash
source ~/.bashrc
```

## 验证

输入 `apt-get in` 然后按 Tab，应该能自动补全为 `apt-get install`。

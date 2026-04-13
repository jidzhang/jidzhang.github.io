---
title: "Debian开启/完善bash_completion"
date: 2013-10-15
draft: false
slug: "enable-bash_completion-debian"
categories:
  - "Linux"
---

真心喜欢Debian系统，系统非常稳定而且各方面的技术材料也丰富。但是有些配置还是需要自己搞定。下面是一个tips：<br/>

**完善bash的自动补全功能，即bash_completion**

（1）为什么做？

因为系统默认的自动补全功能不完整，甚至是没有。

（2）怎么做？下面是具体的步骤：

* 安装bash_completion包：
```bash
apt-get install bash_completion
```

如果已安装可跳过。

* 修改bashrc，如果你能获得root权限，那么你可以直接改 `/etc/bash.bashrc`，把
```bash
#if ! shopt -oq posix; then
#   if [ -f /usr/share/bash-completion/bash_completion ]; then
#      . /usr/share/bash-completion/bash_completion
#   elif [ -f /etc/bash_completion ]; then
#      . /etc/bash_completion
#   fi
# fi
```

每一行前面的#去掉，然后保存重新登录一下系统即可。
如果不能获得root权限，那就把上面的一段文字复制到 `~/.bashrc`。

（3）最后，其实我们什么都没有做就实现了这个很爽的东西，我们只是把默认关闭的东西打开罢了。*Enjoy your Debian OS ...*

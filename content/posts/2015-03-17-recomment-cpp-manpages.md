---
title: "强烈推荐：C++ manpages -- C++在线帮助文档"
date: 2015-03-17
draft: false
slug: "recomment-cpp-manpages"
categories:
  - "Geek"
---

## C++ manpages -- C++在线帮助文档

> C++ manul pages generater：
>
>    cppman generates C++ manual pages from cplusplus.com and provide a man-like interface to view man pages.
>
> PPA can be found here:https://launchpad.net/~aitjcize/+archive/manpages-cpp

此manpages与Linux自带的manpage一脉相承，虽然使用方式上稍有不同，但是文档格式非常一致。

此manpages是C++程序员的必备工具，虽然还有一个与此很相似的文档libstdc++6-4.4-doc，但是由于那个文档的格式与Linux中的manpage差别较大，使用很不方便。因此，这里极力推荐此manpage。

如下（本文主要在Ubuntu下操作）：

（1）向系统中添加PPA（即源），并更新
```bash
sudo add-apt-repository ppa:aitjcize/manpages-cpp
sudo apt-get update
```

（2）安装程序 manpages-cpp
```bash
sudo apt-get install manpages-cpp
```

（3）从Cplusplus网站上获取数据：这一步比较漫长，先去泡杯咖啡吧
```bash
cppman -c
```

（4）畅游cppman
```bash
cppman cout
cppman iterator
```

 （5）获取帮助
```bash
cppman  --help
```
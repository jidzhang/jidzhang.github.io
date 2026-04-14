---
title: "Linux 安装 C++ man pages"
date: 2015-03-15
draft: false
slug: "linux-install-cpp-man-pages"
categories:
  - "Linux"
---

本篇介绍在ubuntu上安装C++在线文档（man pages），下篇将介绍[C++在线文档的使用](../16/howto-cpp-man-pages.html)。需要注意的一点是：在这里的方法有别于通常的 man func_name，因此需要单独说明，以免安装后也不知道怎么用。

另外，为了达到最好的使用效果，请首先把C的开发文档提前安装上吧，命令：
```bash
sudo apt-get update
sudo apt-get install manpages-dev glibc-doc
```

然后安装C++的开发文档，命令通常是
```bash
sudo apt-get install libstdc++6-4.4-doc
```

上面的4.4是gcc的版本，可以是4.2、4.3、4.4, 4.4是最新的。至于软件库中有无4.4，请参见下文的说明，使用命令查询：
```bash
apt-cache search libstdc++|grep "doc"
```

以决定安装4.2、4.3或4.4

下面是英文原文：
>
```bash
It’s so easy to search for any C related programming functions. All you need is to search the man pages. Take for example, if you want to know more about printf, simply type
`$ man 2 printf`
But that’s not the case with C++ functions. Every time, you have to go to the web for any doubts related to C++ functions. There are two ways by which you can search for documentation related to C++ functions.
    Man pages for C++ functions
    HTML documentation
Both of the above are available from a single package. To install C++ documentation
`$ sudo apt-get install libstdc++6-4.4-doc`
This installs both the man pages and HTML documentation.
The HTML documentation is found in the following path:file:///usr/share/doc/libstdc++6-4.4-doc/libstdc++/html/index.html
If you are struggling to search C++ man pages, ReferHow to search C++ man pages
Now there may be case that when you are referring this article the documentation 6.4.4 version is no longer available.
In that case, follow the following steps
`$ apt-cache search libstdc++|grep"doc"`
libstdc++6-4.4-doc - The GNU Standard C++ Library v3 (documentation files)libstdc++6-4.1-doc - The GNU Standard C++ Library v3 (documentation files)libstdc++6-4.3-doc - The GNU Standard C++ Library v3 (documentation files)
Search for the libstdc++ docs as shown above and install the version you want, preferably the latest ones (as shown above)
`$ sudo apt-get install libstdc++6-4.4-doc`
```


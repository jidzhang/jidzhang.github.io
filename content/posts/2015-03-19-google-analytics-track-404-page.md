---
title: "使用Google Analytics跟踪404页面"
date: 2015-03-19
draft: false
slug: "google-analytics-track-404-page"
categories:
  - ""
---

> 2026-04-13 修正错误

404页面是当访问者输入了错误的地址或者访问了被删除的页面时，服务器返回的错误页面（404 HTTP 状态代码）。这个页面除了告诉访问者页面不存在以外，不提供任何有价值的信息。访问者可能就此离开网站。

了解404页面的信息非常有用，可以发现访问者要查找的内容和推介来源，有助于网站补充新的内容并修复有问题的链接。如何使用Google Analytics来追踪并显示404页面的情况？Google Analytics的官方博客介绍了[一个简单的方法](http://analytics.blogspot.com/2006/09/tip-tracking-404-pages.html)，使用Google Analytics可以跟踪网站的404页面错误。

(1) 将网站的Google Analytics追踪代码添加到404 页面里。

(2) 修改404页面的Google Analytics代码，将代码修改为以下形式：
```
<script type="text/javascript"
src="http://www.google-analytics.com/urchin.js">
</script>
<script type="text/javascript">
_uacct = "xxxxx-x";
urchinTracker("/404.html?page=" + _udl.pathname + _udl.search);
</script>
```

(3) 在热门内容报告中即可查看/404.html页面的报告，里面的信息包括出现错误的URL地址，还会显示访问者上一个访问的页面（推介来源）。通过这些信息，可以及时检查相关页面，修改错误链接。

转自：[月光博客](http://www.williamlong.info/archives/2699.html)

英文原文：[Tip: Tracking 404 Pages](http://analytics.blogspot.com/2006/09/tip-tracking-404-pages.html)

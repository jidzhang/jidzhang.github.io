---
title: "Firefox 开启或关闭 javascript"
date: 2015-04-06
draft: false
slug: "firefox-enable-or-disable-javascript"
categories:
  - "Geek"
---

升级到Firefox23.0后突然发现javascript被禁用了，浏览很多网站都出问题，而且

Firefox23.0在首选项中取消了javascript的设置项。

郁闷了一天后，突然想到一个稳妥的办法，改config，步骤如下：

1. 在地址栏中输入about:config, 然后选“我保证会小心”，进入设置项页面

2. 在出现的搜索栏内输入javascript，在下面的列表中会看到javascript.enabled被设置为false
![firefox confiure](/images/firefox-enable-js.jpeg)

3. 双击刚才看到的条目，把选项改为true！

要禁用（你确定？）javascript，则再双击一次，改为false即可！

不需要重启Firefox，现在javascript已经被启用了！

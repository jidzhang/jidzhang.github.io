---
title: "undefined reference to 'pthread_mutex_trylock' 解决办法"
date: 2015-04-30
draft: false
slug: "fix-undefined-reference-to-pthread_mutex_trylock"
categories:
  - "cpp"
---

这个编译错误的解决办法：


在命令行中(或编译选项中)链接pthread库，即 -lpthread

比如： gcc main.c -o test_main -lpthread

即可

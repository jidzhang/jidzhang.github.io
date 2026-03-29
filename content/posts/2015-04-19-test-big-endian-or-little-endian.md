---
title: "测试处理器是大端序还是小端序(Big Endian or Little Endian)"
date: 2015-04-19
draft: false
slug: "test-big-endian-or-little-endian"
categories:
  - "Geek"
---

下面的程序可以用来测试是大端序还是小端序：

```c

/*
 * How can I determine whether a machine'sbyte orderis big-endian or little-endian?
 * */

#include <stdio.h>

int main(int argc, char const *argv[])
{
    /* method 1 */
    int x = 1;
    if(*(char *)&x == 1)
        printf("little-endian\n");
    else
        printf("big-endian\n");

    /* method 2 */
    {
        union {
            int i;
            char c[sizeof(int)];
        } unx;

        unx.i = 1;
        if(unx.c[0] == 1)
            printf("little-endian\n");
        else
            printf("big-endian\n");
    }

    return 0;
}
```

什么是大端序/小端序啊？ 出处是“爱丽丝梦游仙境”里的鸡蛋的吃法，从小端开始吃还是从大端开始吃，如下图：
![how to eat egg](/images/bigendian-or-littleendian.jpeg)
 

参考： [wikipedia](http://en.wikipedia.org/wiki/Big-endian)



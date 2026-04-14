---
title: "测试处理器是大端序还是小端序(Big Endian or Little Endian)"
date: 2015-04-19
draft: false
slug: "test-big-endian-or-little-endian"
categories:
  - "Geek"
---

下面的程序提供了两种方法判断处理器字节序：

**方法一：指针强制转换**
```c
#include <stdio.h>

/* 返回 1 表示小端序，0 表示大端序 */
int is_little_endian(void)
{
    int x = 1;
    return *(char *)&x == 1;
}

int main(void)
{
    if (is_little_endian())
        printf("little-endian\n");
    else
        printf("big-endian\n");
    return 0;
}
```

**方法二：利用 union**
```c
#include <stdio.h>

/* 返回 1 表示小端序，0 表示大端序 */
int is_little_endian_union(void)
{
    union {
        int i;
        char c[sizeof(int)];
    } unx;

    unx.i = 1;
    return unx.c[0] == 1;
}

int main(void)
{
    if (is_little_endian_union())
        printf("little-endian\n");
    else
        printf("big-endian\n");
    return 0;
}
```

什么是大端序/小端序啊？ 出处是“爱丽丝梦游仙境”里的鸡蛋的吃法，从小端开始吃还是从大端开始吃，如下图：
![how to eat egg](/images/bigendian-or-littleendian.jpeg)
 

参考： [wikipedia](http://en.wikipedia.org/wiki/Big-endian)



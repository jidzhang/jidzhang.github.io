---
title: "Source Insight 3.5.x 序列号算法分析"
date: 2013-05-21
draft: false
slug: "source-insight-35x-keygen"
categories:
  - "逆向工程"
---

> ⚠️ 本文仅用于逆向工程学习和算法分析交流，请勿用于商业用途。

## 算法概述

Source Insight 3.5.x 的序列号格式为 `SI3US-XXXXXX-XXXXX`，由两部分组成：

1. **中间数（6 位）**：随机生成，但排除一些特定值（mask 表中的 32 个值、全相同数字等）
2. **校验码（5 位）**：由中间数通过固定算法计算得出

## 核心算法

```c
/*
 * Source Insight 3.5.x 序列号算法
 * 可用 VC/GCC 编译
 */
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <string.h>
#include <time.h>

/* 提取数字 v 的第 i 位 */
unsigned int digit(unsigned int v, unsigned int i)
{
    v = v % (int)pow(10, i);
    return v / (int)pow(10, i - 1);
}

/* 生成大范围随机数 */
long lrand()
{
    if (RAND_MAX == 0x7FFF)
        return (long)((rand() << 16) | rand());
    else
        return rand();
}

int main()
{
    /* 被排除的中间数值 */
    const unsigned int mask[32] = {
        0x3039, 0x1E240, 0x5464E, 0x6F855, 0x8AA52, 0xA5BF5, 0x980D0,
        0xBADD0, 0x980D0, 0x30448, 0x30379, 0x8AF93, 0x30379, 0x7B9,
        0xF2F90, 0x9FF66, 0x8AA52, 0xF3A0, 0x30379, 0x8AA52, 0x62AF,
        0x2AD, 0x71FDC, 0x5A124, 0xFFB2, 0xB96D8, 0x2B18E, 0x4CDE4,
        0x71FDC, 0x1068C, 0x765C3A63, 0x745C3533
    };
    /* 校验码计算的变换表 */
    const int i_dat[10] = {0x96, 0x95, 0x10, 0x23, 7, 0x15, 8, 3, 16, 17};

    srand((unsigned int)time(NULL));

    while (1) {
        unsigned int d = 0;
        unsigned int mid_number = 0;
        char resp[128];

        /* 生成合法的 6 位中间数 */
        while (1) {
            d = lrand() % 1000000;
            if (d < 100000 || d % 111111 == 0)
                continue;
            /* 排除 mask 表中的值 */
            int i;
            for (i = 0; i < 32; ++i) {
                if (mask[i] == d) break;
            }
            if (i == 32) break;
        }
        mid_number = d;

        /* 计算校验码 */
        int i;
        for (i = 0; i < 6; ++i) {
            d = (i_dat[i] ^ (digit(mid_number, 6 - i) + 48)) + 4 * d;
        }
        d = d % 100000;
        if (d < 10000)
            continue;

        printf("Serial number: SI3US-%d-%d\n", mid_number, d);

        printf("Generate another? (y/n): ");
        memset(resp, 0, 128);
        scanf("%s", resp);
        if (resp[0] != 'y' && resp[0] != 'Y') {
            printf("Good Bye!\n");
            break;
        }
    }

    return 0;
}
```

## 算法要点

1. 中间数必须是 6 位（100000-999999），且不能全相同
2. 32 个 mask 值是被排除的特定数字
3. 校验码通过中间数的每一位与 `i_dat` 表异或运算后累加得出
4. 校验码必须 ≥ 10000（即 5 位数）

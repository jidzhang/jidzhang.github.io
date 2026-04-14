---
title: "最大公约数（GCD）的两种 C 语言实现"
date: 2015-01-15
draft: false
slug: "calculate-gcd-in-c"
categories:
  - "Code"
---

## 一、辗转相除法（欧几里得算法）

### 数学原理

**定理：** `gcd(a, b) = gcd(b, a % b)`，其中 `a ≥ b`。

**证明：**

设 `a = k * b + r`（其中 `k = a / b`，`r = a % b`），则：

- 若 `d` 能整除 `a` 和 `b`，则 `d` 也能整除 `r = a - k * b`
- 若 `d` 能整除 `b` 和 `r`，则 `d` 也能整除 `a = k * b + r`

因此 `(a, b)` 和 `(b, a % b)` 的公约数完全相同，最大公约数也相同。不断递归直到余数为 0，此时的除数即为 GCD。

### 代码实现

```c
#include <stdio.h>

int gcd(int a, int b)
{
    while (b != 0) {
        int r = a % b;
        a = b;
        b = r;
    }
    return a;
}

int main()
{
    printf("gcd(56, 108) = %d\n", gcd(56, 108));  // 输出 4
    return 0;
}
```

---

## 二、Stein 算法

### 为什么需要 Stein

辗转相除法需要取模运算，对于**超大整数**（超出 CPU 原生整数范围），取模运算代价很高。Stein 算法只用加减法和移位，避免了大数除法。

### 数学原理

利用以下性质递归求解：

1. `gcd(a, 0) = a`
2. `gcd(2a, 2b) = 2 * gcd(a, b)` — 两个偶数，提取公因子 2
3. `gcd(2a, b) = gcd(a, b)`（b 为奇数）— 只有一个偶数，偶数除以 2
4. `gcd(a, b) = gcd(|a - b| / 2, min(a, b))`（a, b 均为奇数）— 差必为偶数

### 代码实现

```c
#include <stdio.h>

int stein_gcd(int a, int b)
{
    if (a == 0) return b;
    if (b == 0) return a;

    // 找到 a 和 b 共有的因子 2
    int k = 0;
    while (((a | b) & 1) == 0) {
        a >>= 1;
        b >>= 1;
        k++;
    }

    // 去掉 a 中剩余的因子 2
    while ((a & 1) == 0)
        a >>= 1;

    do {
        // 去掉 b 中剩余的因子 2
        while ((b & 1) == 0)
            b >>= 1;

        // 保证 a >= b
        if (a > b) {
            int t = a;
            a = b;
            b = t;
        }
        b = b - a;
    } while (b != 0);

    return a << k;
}

int main()
{
    printf("gcd(56, 108) = %d\n", stein_gcd(56, 108));  // 输出 4
    return 0;
}
```

---

## 对比

| 算法 | 核心运算 | 适用场景 |
|------|---------|---------|
| 辗转相除法 | 取模（除法） | 普通整数，代码简洁 |
| Stein 算法 | 移位、加减 | 大整数运算，无硬件除法支持的场景 |

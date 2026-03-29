---
title: "最大公约数 C 语言实现"
date: 2015-01-15
draft: false
slug: "calculate-gcd-in-c"
categories:
  - "Code"
---

最大公约数：Greatest Common Divisor，简写为gcd。

这里提供求两个整数最大公约数的计算机算法

1. 辗转相除法（或称欧几里得算法）

2. Stein算法（该算法解决了大数相除的困难）

------------

（1）辗转相除法（或称欧几里得算法）

欧几里得算法的依据是数学定理：gcd(y,x)=gcd(y,y%x) (其中y>=x).

下面给出该定理的证明：

	由于 y >= x, 可以表示为
	y = k*x + b, 其中
	k = y/x, b = y%x, 即k和b分别表示商和余数。
	下面，
	如果一个数（d）能同时被y 和 x 整除，
	记为 d|y, d|x,
	由 b = y – k*x, 知道 b也能被d整除，即 d|b
	同理，如果有整数D能被x 和 b 整除，即 D|x, D|b,
	又由y = k*x + b, 知道 y也能被D整除，即 D|y.

	现在由上面的推导知道（y, x）和（x, b）的公约数是一样的，即
	（y, x）和（x, y%x）的公约数是相同的，那么他们的最大公约数也相同，即
	gcd(y,x)=gcd(y,y%x)。

欧几里得算法就依据这个公式：gcd(y,x)=gcd(y,y%x)，直到出现余数为零，这时的除数就是最后的最大公约数。

```c

/*C语言实现*/
/*辗转相除法*/

#include <stdio.h>

void swap(int *a, int *b)
{
    int tmp;
    tmp = *a;
    *a = *b;
    *b = tmp;
}

int gcd(int a, int b)
{
    if (a == 0)
        return b;
    if (b == 0)
        return a;
    if (a < b)
        swap(&a,&b);

    int result = a%b;
    while(result != 0)
    {
        a = b;
        b = result;
        result = a%b;
    }
    return b;
}

int main()
{
    int m, n;
    m = 56; n = 108;
    printf("The greatest common divisor of %d and %d is: %d\n",m,n,gcd(m,n));
    return 0;
}
```

（2）Stein算法

Stein算法利用递归算法，避免了大数直接相除，对于大数运算有深远影响。

Stein算法利用下面的性质：

    gcd(a,a) = a，也就是一个数和他自身的公约数是其自身
    gcd(ka,kb) = k gcd(a,b)，也就是最大公约数运算和倍乘运算可以交换，特别的，当k=2时，说明两个偶数的最大公约数必然能被2整除

```c

/*C语言实现*/
/*Stein算法*/
#include <stdio.h>
int gcd(int a,int b)
{
    if (a < b)
    {
        int tmp = a;
        a = b;
        b = tmp;
    }
    if (b == 0)
        return a;
    if (a%2==0 && b%2==0)
        return 2*gcd(a/2,b/2);
    if (a%2==0)
        return gcd(a/2,b);
    if (b%2==0)
        return gcd(a,b/2);
    return gcd((a+b)/2,(a-b)/2);
}

int main()
{
    int m, n;
    m = 56; n = 108;
    printf("The greatest common divisor of %d and %d is: %d\n",m,n,gcd(m,n));
    return 0;
}
```

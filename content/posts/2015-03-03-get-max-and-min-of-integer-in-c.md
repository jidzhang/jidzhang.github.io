---
title: "获取整数的最大值最小值的宏定义及可能出现的问题"
date: 2015-03-03
draft: false
slug: "get-max-and-min-of-integer-in-c"
categories:
  - "Code"
---

在C/C++编程中时常需要使用整数的最大值最小值，通常这两个常用是跟平台和操作系统有关的，不同的平台会有不同的值，因此可移植的办法就是使用库函数提供的常量定义。

（1）类似的常量定义在limits.h和float.h头文件中，可以查看源文件获取类似常量的使用办法。在头文件中，整数的最值通常是这样的名字：INT_MAX, INT_MIN，直接使用即可。

（2）当然这两个最值完全可以通过编程实现：

	#define MAX_INT ((unsigned)(-1)>>1)
	#define MIN_INT (~MAX_INT)

但是，这两个宏仅仅是没有类别的符号，在使用的时候会陷入困境。看下面这段C++程序，输出结果出乎意料。

	#include <iostream>
	#include <limits>

	#define MAX_INT ((unsigned)(-1)>>1)
	#define MIN_INT (~MAX_INT)

	int main()
	{
	    std::cout << "max_int: " << MAX_INT << "\n"
	        << "min_int: " << MIN_INT << std::endl;
	}

输出结果是

	max_int: 2147483647
	min_int: 2147483648

结果怎么会是这样呢？

问题就出在：输出`MIN_INT`时，由于`MIN_INT`仅仅是个符号，在输出给cout时就按照Cpp的规则以长整数输出了，因此正确的办法是

    cout << "max_int: " << (int)MAX_INT << "\n"
        << "min_int: " << (int)MIN_INT << endl;

当然最好的办法还是不要使用`#define`这个宏，不安全。

（3）针对上面的问题，一个比较好的解决办法是，直接定义常量：

	const int MAX_INT = ((unsigned)(-1))>>1;
	const int MIN_INT = ~MAX_INT;

示范代码如下：

	#include <stdio.h>
	int main()
	{
	    const int MAX_INT = ((unsigned)(-1))>>1;
	    const int MIN_INT = ~MAX_INT;

	    printf("Max_int : %d\nMin_int : %d\n", MAX_INT, MIN_INT);
	    return 0;
	}

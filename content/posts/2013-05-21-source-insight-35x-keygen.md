---
title: "source insight 3.5.x 算号器源代码"
date: 2013-05-21
draft: false
slug: "source-insight-35x-keygen"
categories:
  - "Hack"
---

下面是本人用C写的source insight 3.5.x 算号器源代码，可以用VC、GCC或在线[CodePad](http://codepad.org/)进行编译尝试：

效果图：
![SI Keygen](/images/2013-05-21-si-keygen.png)

可执行程序：
[SI-Keygen-CMD](https://dl.dropboxusercontent.com/u/6893139/exe/si-keygen.exe)

源代码：

```c

/*
* By Apneng.Net @2013.05.21
* 声明：以下程序仅仅用于测试二进制可执行文件，
* 并且仅仅用于同行间交流学习。
* 请勿用于商业目的。
*/
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <string.h>
#include <time.h>

unsigned int digit(unsigned int v, unsigned int i)
{
	v = v%(int)pow(10, i);
	return v/(int)pow(10, i-1);
}
/*big random number*/
long lrand()
{
	if (RAND_MAX == 0x7FFF)
		return (long)((rand() << 16)| rand());
	else
		return rand();
}

int main()
{
	const unsigned int mask[32] = {
		0X3039, 0X1E240, 0X5464E, 0X6F855, 0X8AA52, 0XA5BF5, 0X980D0,
		0XBADD0, 0X980D0, 0X30448, 0X30379, 0X8AF93, 0X30379, 0X7B9,
		0XF2F90, 0X9FF66, 0X8AA52, 0XF3A0, 0X30379, 0X8AA52, 0X62AF,
		0X2AD, 0X71FDC, 0X5A124, 0XFFB2, 0XB96D8, 0X2B18E, 0X4CDE4,
		0X71FDC, 0X1068C, 0X765C3A63, 0X745C3533
	};
	const int i_dat[10] = {0x96, 0x95, 0x10, 0x23, 7, 0x15, 8, 3, 16, 17};
	srand((unsigned int)time(NULL));

	while (1) {
		unsigned int d = 0;
		unsigned int mid_number = 0;
		char resp[128];
		int i = 0;
		while(1) {
			d = lrand()%1000000;
			if (d < 100000 || d%111111 == 0)
				continue;
			for (i = 0; i < 32; ++i) {
				if (mask[i] == d)
					break;
			}
			if (i==32)
				break;
		} 
		mid_number = d;

		for (i=0; i<6; ++i) {
			d = (i_dat[i]^(digit(mid_number, 6-i)+48)) + 4*d;
		}
		d = d%100000;
		if (d < 10000)
			continue;

		printf("Serial number: SI3US-%d-%d\n", mid_number, d);

		// try again
		printf("Do you want to have a try, again(y|n)? ");
		
		memset(resp, 0, 128);
		scanf("%s", resp);
		fflush(stdin);
		if (resp[0]=='y' || resp[0]=='Y') {
			continue;
		} else {
			printf("Good Bye!\n");
			break;
		}
	}

	return 0;
}

```

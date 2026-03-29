---
title: "判断质数的小算法"
date: 2015-03-22
draft: false
slug: "judge-prime"
categories:
  - "Code"
---
> 质数，又称素数，指在一个大于1的自然数中，除了1和此整数自身外，无法被其他自然数整除的数(也可定义为包含1和本身的因子等于2个)。比1大但不是素数的数称为合数。1和0既非素数也非合数。素数在数论中有着很重要的地位。(From [wikipedia](http://zh.wikipedia.org/wiki/%E7%B4%A0%E6%95%B0))

下面提供判断一个整数是否是质数的简单C实现:

{% highlight c %}
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
int Prime(int num)
{
    num = abs(num);
    if ((num==0)&&(num==1))  return 0;
    /*int k = num/2;*/
    int k =(int)sqrt(num);
    while((k>1)&&(num%k))
    { --k; }
    return 1==k;
}
int main()
{
    int num;
    printf("Please input an integer (Ctrl-Z to exit) : ");
    while(scanf("%d",&num))
    {
        if (Prime(num)){
            printf("\n%d is a Prime.\n",num);
        }
        else
            printf("\n%d is not a Prime.\n",num);

        printf("\nPlease input an integer (Ctrl-Z to exit) : ");
    }
}
{% endhighlight %}

算法逻辑：
    Prime函数用来判断某个整数num是否是质数，是则返回非零整数，否则返回零。    Prime函数内部，首先定义一个num的中间数k，可以是num的一半，可以是num的平方根，当然也可以是原数，这里选取平方根，因为这样做效率更高。然后拿这个数k 与 num相除，同时k递减。如果num是质数，那么k在递减的过程中都不会整除num，直到k==1 ; 而如果num不是质数，那么就会找到num的一个非1的因数。

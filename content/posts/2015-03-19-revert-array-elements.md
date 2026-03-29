---
title: "把数组的前k位逆置:递归算法和迭代算法"
date: 2015-03-19
draft: false
slug: "revert-array-elements"
categories:
  - "Code"
---

## 把数组的前k位逆置

{% highlight c %}
/*
* 逆置数组arr的前k位:
* (1)递归实现
* (2)迭代实现
* PS.通常递归算法能解决一些不能直接解决的问题，
* 但是递归算法通常要做出效率上的牺牲
* By Guv 2011.06.04
* */
#include <stdio.h>
/*递归实现*/

void invert(int arr[], int k)
{
    if(k>1){
        invert(arr+1,k-2);
        int tmp = arr[0];
        arr[0] = arr[k-1];
        arr[k-1] = tmp;
    }
}

/*迭代*/
/*
void invert(int arr[], int k)
{
    int tmp=0, mid = k/2;
    if (k<2)    return;
    for(int i=0;i<mid;++i){
        tmp = arr[i];
        arr[i] = arr[k-1-i];
        arr[k-1-i] = tmp;
    }
}
*/

/*testing*/
int main()
{
    int array[] = {1,2,5,9,8,7,6,3,5,6,7,8,9,5,6,4,1,5,2,6,3,5};
    int num = sizeof(array)/sizeof(int);

    printf("before inverted:");
    for(int i=0;i<num;++i){
        if (i%5==0)
            printf("\n");
        printf("%d, ", array[i]);
    }

    invert(array, num);

    printf("\nafter inverted:");
    for(int i=0;i<num;++i){
        if (i%5==0)
            printf("\n");
        printf("%d, ", array[i]);
    }
}
{% endhighlight %}

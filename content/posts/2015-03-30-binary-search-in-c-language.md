---
title: "二分法查找C语言实现"
date: 2015-03-30
draft: false
slug: "binary-search-in-c-language"
categories:
  - "Code"
---

在对线性表的操作中，经常需要查找一个元素在表中的位置。通常最坏的办法是遍历整个线性表，以找到某元素的位置或者不在表中。
但是如果线性表是有序的，比如由小到大递增（完全可以是自定义的比较方法），那么在这种情况下使用二分查找法会是首选。当然，这么重要的函数，一般的库都会提供，比如：

> C provides bsearch() in its standard library.
> 
> C++'s STL provides algorithm functions binary_search, lower_bound and upper_bound.
> 
> Java offers a set of overloaded binarySearch() static methods in the classes Arrays and Collections in the standard > java.util package for performing binary searches on Java arrays and on Lists, respectively. They must be arrays of > primitives, or the arrays or Lists must be of a type that implements the Comparable interface, or you must specify a > custom Comparator object.
> 
> Microsoft's .NET Framework 2.0 offers static generic versions of the binary search algorithm in its collection base > classes. An example would be System.Array's method BinarySearch<T>(T[] array, T value).
> 
> Python provides the bisect module.
> 
> COBOL can perform binary search on internal tables using the SEARCH ALL statement.
> 
> Perl can perform a generic binary search using the CPAN module Search::Binary.[7]
> 
> Go's sort standard library package contains functions Search, SearchInts, SearchFloat64s, and SearchStrings, which > implement general binary search, as well as specific implementations for searching slices of integers, floating-point > numbers, and strings, respectively.[3]
> 
> For Objective-C, the Cocoa framework provides the NSArray -indexOfObject:inSortedRange:options:usingComparator: method > in Mac OS X 10.6+. Apple's Core Foundation C framework also contains a CFArrayBSearchValues() function.
> 
> (以上文字摘自wikipedia)

但是，正如“编程珠玑”语言的，90%的程序员却不能在第一次写出无bug的二分查找算法，虽说二分查找的思想很简单，但一实现就不免有疏漏。下面提供一段本人用C写的二分查找算法。
```c

#include <stdio.h>
/*
* array_num 表示数组的元素个数
* 本算法只是实现了递增排序，即operator <
*/
void sort_int(int *array, int array_num)
{
    for (int i=0;i<array_num;++i)
    {
        for (int j=0;j<array_num-1-i;++j)
        {
            if (array[j] > array[j+1])
            {
                int tmp = array[j];
                array[j] = array[j+1];
                array[j+1] = tmp;
            }
        }
    }
}

/*
* if found, return index; else return -1
* 注意：搜寻区间是[low_index,high_index]，即闭区间
*/
int b_search(int *array, int low_index, int high_index, int key)
{
    while(low_index<=high_index)
    {
        int mid = (low_index+high_index)/2;
        if (key==array[mid])
            return mid;
        else if (key < array[mid])
            high_index=mid-1;
        else
            low_index=mid+1;
    }
    return -1;
}

/*test program*/
int main()
{
    int array[] = {2, 2, 4, 5, 6, -1, 4, 5, 6, 7, 8, 8};
    int num=sizeof(array)/sizeof(int);

    sort_int(array, num); //sorting
   
    if (b_search(array, 0, num-1, 7)>=0) //searching
        printf("found\n");
    else
        printf("not found\n");

    return 0;
}
```

观察上面的b_search()函数，可以看到用递归的方式实现也会是一个很好的办法。

递归实现：
```c
#include <stdio.h>

/*
* array_num 表示数组的元素个数
* 本算法只是实现了递增排序，即operator <
*/
void sort_int(int *array, int array_num)
{
    for (int i=0;i<array_num;++i)
    {
        for (int j=0;j<array_num-1-i;++j)
        {
            if (array[j] > array[j+1])
            {
                int tmp = array[j];
                array[j] = array[j+1];
                array[j+1] = tmp;
            }
        }
    }
}

/*
* if found, return index; else return -1
* 注意：搜寻区间是[low_index,high_index]，即闭区间
*/
int b_search(int *array, int low_index, int high_index, int key)
{
    if (low_index > high_index)
        return -1;
    else
    {
        int mid = (low_index+high_index)/2;
        if (key == array[mid])
            return mid;
        else if (key < array[mid])
            return b_search(array, low_index, mid-1, key);
        else
            return b_search(array, mid+1, high_index, key);
    }
}

/*test program*/
int main()
{
    int array[] = {2, 2, 4, 5, 6, -1, 4, 5, 6, 7, 8, 8};
    int num=sizeof(array)/sizeof(int);

    sort_int(array, num); //sorting
   
    if (b_search(array, 0, num-1, 7)>=0) //searching
        printf("found\n");
    else
        printf("not found\n");

    return 0;
}
```

参考资料：[Binary search algorithm (From wikipedia)](http://en.wikipedia.org/wiki/Binary_search_algorithm)

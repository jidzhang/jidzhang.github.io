---
title: "用C语言实现一个简单链表数据结构"
date: 2015-03-14
draft: false
slug: "implement-link-struct-in-c"
categories:
  - "Code"
---
## 用C语言实现一个简单链表数据结构
```c
/*
*  本程序实现了一个简单的单向链表，其中
*  （1）函数 first_insert()的功能是在已知链表的首表元之前插入一个指定值的表元
*  （2）函数 reverse_copy()的功能是把已知链表，按照逆序复制出一个新链表
*  （3）函数 print_link()用来输出链表中各表元的值
*  （4）函数 free_link()用来释放链表的全部表元
*   By linccn 2011.06.06
* */
#include <stdio.h>
#include <malloc.h>

typedef struct node{
    int val;
    struct node *next;
}NODE;

void first_insert(NODE ** p, int v)
{
    NODE *q = (NODE *)malloc(sizeof(NODE));
    q->val = v;
    q->next = *p;

    *p = q;
}

NODE *reverse_copy(NODE *p)
{
    NODE *u;
    for (u=NULL;p!=NULL;p=p->next)
        first_insert(&u, p->val);

    return u;
}

void print_link(NODE *p)
{
    for (;p!=NULL;p=p->next)
        printf("%d, ", p->val);
    printf("\n");
}

void free_link(NODE *p)
{
    NODE *u;
    while(p!=NULL){
        u = p->next;
        free(p);
        p = u;
    }
}

int main()
{
    NODE *link1, *link2;
    int i;
    link1 = NULL;
    for(i=1;i<10;++i)
        first_insert(&link1, i);

    link2 = reverse_copy(link1);
    print_link(link2);
    free_link(link1);
    free_link(link2);

    return 0;
}
```

---
title: "用C语言实现简单的链式栈结构"
date: 2015-03-18
draft: false
slug: "implement-linkstack-in-c"
categories:
  - "Code"
---
## C语言实现简单的链式栈结构

```c
/*
 *  用链式存储结构实现的栈称为链栈。若链栈元素的数据类型为valuetype,以LinkStack记链栈结构，其类型定义为：
 * typedef struct node
 * {
 *    valuetype data;
 *    struct node * next;
 * } StackNode, *LinkStack;
 *
 * 由于栈的主要操作都是在栈顶进行的，因此把链表的头部作为栈顶。设top为栈顶指针，即：LinkStack top.
 * 下面为各函数的功能说明：
 * （1）LinkStack Init_LinkStack() : 建立并返回空的链栈
 * （2）int Empty_LinkStack(LinkStack top) : 判断top所指链栈是否为空
 * （3）LinkStack Push_LinkStack(LinkStack top, valuetype x) : 将数据x压入top所指的栈顶，并返回新栈指针
 * （4）LinkStack Pop_LinkStack(LinkStack top, valuetype *x) : 弹出top所指链栈的栈顶元素x,返回新栈指针
 *
 */

#include <stdio.h>
#include <malloc.h>

typedef int valuetype;
typedef struct node
{
    valuetype data;
    struct node * next;
} StackNode, *LinkStack;

LinkStack Init_LinkStack()
{
    return NULL;
}

int Empty_LinkStack(LinkStack top)
{
    if (top==NULL)  return 1;
    else    return 0;
}

LinkStack Push_LinkStack(LinkStack top, valuetype x)
{
    StackNode *s = (StackNode *)malloc(sizeof(StackNode));
    s->data = x;
    s->next = top;
    return s;
}

LinkStack Pop_LinkStack(LinkStack top, valuetype *x)
{
    StackNode *p;
    if (top==NULL)  return NULL;
    else {
        *x = top->data;
        p = top->next;
        free(top);
        return p;
    }
}

int main()
{
    LinkStack linkstack = Init_LinkStack();
    int i=0;
    int x=0;
    for (i=0;i<10;++i)
    {
        linkstack = Push_LinkStack(linkstack, i);
    }
    while(linkstack)
    {
        linkstack = Pop_LinkStack(linkstack, &x);
        printf("%d ",x);
    }
    printf("\n");
}

```

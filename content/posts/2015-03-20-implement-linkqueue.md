---
title: "用C语言实现简单链式队列结构"
date: 2015-03-20
draft: false
slug: "implement-linkqueue"
categories:
  - "Code"
---

##用C语言实现简单链式队列结构

{% highlight c %}
/*
 * 用链式结构实现的队列称为链队。根据队列的FIFO原则，为了操作上的方便，可以用带头指针font和尾指针rear的单链表来实现队列。链队结构描述：
 * typedef struct node
 * {
 *     valuetype data;
 *     struct node *next;
 * }QNode;   [>链队结点的类型<]
 * typedef struct
 * {
 *     QNode *font, *rear;
 * }LQueue;    [>将头尾指针封装在一起的链队<]
 *
 * 设q是一个指向链队的指针，即LQueue *q. 各函数的功能如下：
 * （1）LQueue * Init_LQueue(): 创建并返回一个带头尾结点的空链队
 * （2）int Empty_LQueue(LQueue *q): 判断链队是否为空
 * （3）void In_LQueue(LQueue *q, valuetype x) : 将数据x压入链队q
 * （4）int Out_LQueue(LQueue *q, valuetype *x) : 弹出链队q的第一个元素x, 若成功则返回1否则返回0
 * By Guv 2011.06.10
 */

/*程序代码：*/

#include <stdio.h>
#include <malloc.h>

typedef int valuetype;

typedef struct node
{
    valuetype data;
    struct node *next;
}QNode;
typedef struct
{
    QNode *font, *rear;
}LQueue;

LQueue * Init_LQueue()
{
    LQueue *q;
    QNode *p;
    q = (LQueue*)malloc(sizeof(LQueue));    /*申请链队指针*/
    p = (QNode*)malloc(sizeof(QNode));  /*申请头尾指针结点*/
    p->next = NULL;

    q->font = q->rear = p;
    return q;
}

int Empty_LQueue(LQueue *q)
{
    if(q->font != q->rear)  return 0;
    else    return 1;
}

void In_LQueue(LQueue *q, valuetype x)
{
    QNode *p = (QNode *)malloc(sizeof(QNode));  /*申请新结点*/
    p->data = x;
    p->next = NULL;
    q->rear->next = p;
    q->rear = p;
}

int Out_LQueue(LQueue *q, valuetype *x)
{
    if(Empty_LQueue(q)) return 0;   /*空链队*/
    else{
        QNode *p = q->font->next;
        *x = p->data;
        q->font->next = p->next;
        free(p);
        if (q->font->next == NULL)
            q->rear = q->font;

        return 1;
    }
}

int main()
{
    LQueue *queue = Init_LQueue();
    for (int i=0;i<10;++i)
    {
        In_LQueue(queue, i);
    }
    while(!Empty_LQueue(queue))
    {
        int k=0;
        Out_LQueue(queue, &k);
        printf("%d ",k);
    }
    printf("\n");
    return 0;
}

{% endhighlight %}

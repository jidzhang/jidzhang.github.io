---
title: "用C语言实现简单循环队列结构"
date: 2015-03-21
draft: false
slug: "implement-circularqueue"
categories:
  - "Code"
---

## 用C实现简单循环队列结构
```c
/*
 * 用C实现简单循环队列结构
 *
 * 循环队列的类型定义如下：
 * typedef struct{
 *     valuetype data[MAXSIZE];    [>数据的存储区<]
 *     int front, rear;     [>队首队尾<]
 *     int num;    [>队列中元素的个数<]
 * }Circular_Queue;
 * 循环队列常用函数如下：
 * （1）Circular_Queue * Init_CirQueue(): 初始化队列
 * （2）int In_CirQueue(Circular_Queue *q, valuetype x): 将元素x插入队列q的队尾，若成功返回1，否则返回0
 * （3）int Out_CirQueue(Circular_Queue *q, valuetype *x): 取出队列q队首位置的元素，若成功返回1，否则返回0
 */

/*程序代码：*/

#include <stdio.h>
#include <malloc.h>

#define MAXSIZE 100
typedef int valuetype;
typedef struct{
    valuetype data[MAXSIZE];    /*数据的存储区*/
    int front, rear;     /*队首队尾*/
    int num;    /*队列中元素的个数*/
}Circular_Queue;

Circular_Queue * Init_CirQueue()
{
    Circular_Queue *q = (Circular_Queue*)malloc(sizeof(Circular_Queue));
    q->front = q->rear = MAXSIZE-1;
    q->num = 0;
    return q;
}

int In_CirQueue(Circular_Queue *q, valuetype x)
{
    if(q->num == MAXSIZE)   return 0;   /*队满，不能入队*/
    else {
        /*q->rear = (q->rear+1)%MAXSIZE;*/
        q->data[q->rear] = x;
        q->rear = (q->rear+1)%MAXSIZE;
        q->num++;
        return 1;
    }
}

int Out_CirQueue(Circular_Queue *q, valuetype *x)
{
    if(q->num == 0) return 0;   /*队空*/
    else{
        *x = q->data[q->front];
        q->front = (q->front+1)%MAXSIZE;
        q->num--;
        return 1;
    }
}

int main()
{
    Circular_Queue *queue = Init_CirQueue();
    int x=0;
    for(int i=0;i<10;++i)
    {
        if(!In_CirQueue(queue, i))
            break;
    }
    while(Out_CirQueue(queue, &x))
        printf("%d ",x);
    printf("\n");

    return 0;
}
```

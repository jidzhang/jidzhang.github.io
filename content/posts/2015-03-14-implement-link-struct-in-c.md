---
title: "C 语言实现单向链表"
date: 2015-03-14
draft: false
slug: "implement-link-struct-in-c"
categories:
  - "Code"
---

实现一个简单的单向链表，包含头插法、逆序复制、遍历打印和释放操作。

## 代码

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node {
    int val;
    struct node *next;
} NODE;

/* 头插法：在链表头部插入新节点 */
void first_insert(NODE **head, int v)
{
    NODE *q = malloc(sizeof(NODE));
    q->val = v;
    q->next = *head;
    *head = q;
}

/* 逆序复制链表，返回新链表头 */
NODE *reverse_copy(NODE *p)
{
    NODE *new_list = NULL;
    for (; p != NULL; p = p->next)
        first_insert(&new_list, p->val);
    return new_list;
}

/* 遍历打印 */
void print_link(NODE *p)
{
    for (; p != NULL; p = p->next)
        printf("%d ", p->val);
    printf("\n");
}

/* 释放整个链表 */
void free_link(NODE *p)
{
    while (p != NULL) {
        NODE *next = p->next;
        free(p);
        p = next;
    }
}

int main(void)
{
    NODE *list1 = NULL, *list2;

    /* 构建链表：9 8 7 6 5 4 3 2 1（头插法导致逆序） */
    for (int i = 1; i < 10; i++)
        first_insert(&list1, i);

    list2 = reverse_copy(list1);  /* 1 2 3 4 5 6 7 8 9 */

    print_link(list1);
    print_link(list2);

    free_link(list1);
    free_link(list2);
    return 0;
}
```

## 关键点

- **头插法**（`first_insert`）：新节点插到链表头部，时间复杂度 O(1)
- **逆序复制**：利用头插法的特性——正序遍历原链表、头插到新链表，自然得到逆序
- **释放链表**：先保存 `next` 指针再 `free` 当前节点，避免访问已释放内存

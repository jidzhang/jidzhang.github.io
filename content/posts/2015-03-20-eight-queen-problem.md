---
title: "八皇后问题递归回溯算法实现"
date: 2015-03-20
draft: false
slug: "eight-queen-problem"
categories:
  - "Code"
---

## 八皇后问题递归回溯算法实现
```c

/*
 * 八皇后问题递归回溯算法实现
 *
 * 八皇后问题或N皇后问题描述为：
 * 求解如何在N*N的棋盘上无冲突地排放N个皇后棋子。其中，皇后的移动方式规定为水平、竖直及45°斜线方向。因此，在任意一个皇后所在位置的水平、竖直和45°方向上都不能出现其他的皇后棋子。
 *
 * 回溯法的基本思想是：在包含问题的所有解的解空间树中，按照深度优先搜索的策略，从根结点出发深度探索解空间树。当探索到某一个结点时，要先判断该结点是否包含问题的解，如果包含，就从该节点出发继续探索下去；如果该结点不包含问题的解，那就说明以该结点为根结点的子树一定不包含问题的最终解，因此要跳过对以该节点为根的子树的系统探索，逐层向其祖先结点回溯。这个过程又叫做解空间树的剪枝操作。
 *
 * 八皇后问题可以有很多解法，其中之一是回溯法。
 * 用回溯法求解八皇后问题，可以按列试探可能的解，一旦该列找不到任何可能的解那么就要向后回溯。
 */

/*程序代码如下：*/

#include <stdio.h>
#define NUM 8

/*棋盘用NUM*NUM的矩阵Q描述，其中，里面的元素如果是1表示可以放置皇后，如果为0表示不能放置皇后，另外，(i,j)表示矩阵的第(i,j)个元素*/

/*判断能否在(i,j)位置放置皇后*/
int isOK(int i, int j, int (*Q)[NUM])
{
    int s=0, t=0;
    for(s=i,t=0;t<NUM;++t)      /*判断行*/
        if(t!=j && Q[s][t]==1)
            return 0;
    for(s=0,t=j;s<NUM;++s)      /*判断列*/
        if(s!=i && Q[s][t]==1)
            return 0;
    for(s=i-1,t=j-1;s>=0&&t>=0;--s,--t)   /*判断左上方*/
        if(Q[s][t]==1)
            return 0;
    for(s=i+1,t=j-1;s<NUM&&t>=0;++s,--t)   /*判断左下方*/
        if(Q[s][t]==1)
            return 0;

/* Thank @ewitt*/
/* 下面的东西没用 */
/*    for(s=i-1,t=j+1;s>=0&&t<NUM;--s,++t)   //判断右上方
 *      if(Q[s][t]==1)
 *          return 0;
 *  for(s=i+1,t=j+1;s<NUM&&t<NUM;++s,++t)   //判断右下方
 *      if(Q[s][t]==1)
 *          return 0;

*/
    return 1;
}

void Queen(int j, int (*Q)[NUM])
{
    if(j==NUM)      /*找到一个解*/
    {
        for(int i=0;i<NUM;++i)
        {
            for(int k=0;k<NUM;++k)
                printf("%d ",Q[i][k]);
            printf("\n");
        }
        printf("\n");
        return;
    }
    else{

        for(int i=0;i<NUM;++i)
        {
            if(isOK(i,j,Q))     /*如果可以*/
            {
                Q[i][j]=1;
                Queen(j+1,Q);   /*深度优先探索解空间树*/
                Q[i][j]=0;
            }
        }
    }
}


/*test program*/
int main()
{
    int Q[NUM][NUM];
    int i=0, j=0;
    for(i=0;i<NUM;++i)
        for(j=0;j<NUM;++j)
            Q[i][j] = 0;   /*初始化*/

    Queen(0,Q);

    return 0;
}
```

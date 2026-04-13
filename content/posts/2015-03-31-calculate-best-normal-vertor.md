---
title: "计算点集或多边形的最佳法向量"
date: 2015-03-31
draft: false
slug: "calculate-best-normal-vertor"
categories:
  - "Code"
---

> 2026-04-13 修正错误

有时，我们希望从一组三个以上的点集求出平面方程，这种点集最常见的例子就是多边形顶点。在这种情况下，这些顶点绕多边形顺指针或逆时针地列出。（顺序很重要，因为要依据它决定哪边是"正面"哪边是"反面"。）

一种糟糕的方式是任选三个连续的点并用这三个点计算平面方程。毕竟所选的三个点可能共线，或接近共线。因为数值精度的问题，这将非常糟糕。或者，多边形可能是凹的，所选的点恰好在凹处，从而构成了逆时针（将导致法向量方向错误）。又或者，多边形上的顶点可能不是共面的，这可能是由数值上的不精确，或错误的生成多边形的方法所引起的。我们真正想要的是从点集中求出"最佳"平面的方法，该平面综合考虑了所有的点。

这里提供一个通用且效率较高的算法，计算一组有序点集（顺时针或逆时针）的最佳法向量。

计算公式，其中Pn+1== P1 ：

![best normal](/images/bestnormal.jpg)

算法实现代码（C++实现）：
```cpp

#include <cstdio>
#include <cmath>
#include <vector>
using std::vector;

struct point
{
    double x;
    double y;
    double z;
    point():x(0),y(0),z(0)
    {}
    point(double i, double j, double k):x(i),y(j),z(k)
    {}
    void normalize()
    {
        double len = sqrt(x*x + y*y + z*z);
        x /= len;
        y /= len;
        z /= len;
    }
    void display()
    {
        printf("x=%5.2f, y=%5.2f, z=%5.2f\n",x,y,z);
    }
};

int main()
{
    vector<point> pts;
    pts.push_back(point(0,100,0));
    pts.push_back(point(10,100,10));
    pts.push_back(point(20,100,100));
    pts.push_back(point(0,100,100));
    
    if(pts.size()<=3)
    {
        printf("oops\n exit!\n");
        return 0;
    }
    point pre = *(pts.end()-1);
    point ans = point();
    vector<point>::iterator itr = pts.begin();
    while(itr!=pts.end())
    {
        point cur = *itr;
        ans.x += (pre.z+cur.z)*(pre.y-cur.y);
        ans.y += (pre.x+cur.x)*(pre.z-cur.z);
        ans.z += (pre.y+cur.y)*(pre.x-cur.x);
        pre = cur;
        ++itr;
    }
    ans.normalize();
    ans.display();
    return 0;

}
```

PS:本算法的通用性较高，甚至可以用于不共面的点集。

当然如果明确知道点集共面，并且能确保点集有序排列（顺时针或逆时针），那么这个算法就很强大了，凸凹多边形都没问题。

另外，三个点的情况很容易解决所以没有在本程序中实现。

---
title: "向map中添加或更新元素的高效方法"
date: 2015-03-25
draft: false
slug: "insert-element-to-map-in-cpp"
categories:
  - "Code"
---

向map中添加或更新元素通常有两个办法：map::operator[] 和 成员函数map::insert()。在数据量很小和元素比较简单的情况的情况下，通常两种方法不会有效率上的差异，但是，一旦数据量很大，或元素很复杂（自定义的比较复杂的类）时，就会出现效率上的差异。因此向map中添加元素通常有这么一个原则：

* 当向map中添加元素时，应优先选用insert(), 而不是operator[];

* 当更新已经在map中的元素时，应优先选用operator[].

首先解决一个问题：insert()更新元素？怎么用啊？确实可以用，就是看着很别扭而且效率不高。看下面的代码
```cpp

map<int,double> m;
m[1]=10.0;   //赋值
m[2]=11.0;   //赋值
m[3]=12.0;   //赋值
m[2]=18.0;  //更新
map<int, double> m2;
m2.insert(make_pair(1,10.0));     //赋值
m2.insert(make_pair(2,11.0));     //赋值
m2.insert(make_pair(3,12.0));     //赋值
m2.insert(make_pair(2,18.0)).first->second = 18;     //更新
```

其次,有必要定义一个函数,实现向map中高效的添加或更新元素.
```cpp
template<typename MapType,typename KeyArgType,typename ValueArgType>
typename MapType::iterator
effecient_add_update(MapType &m, KeyArgType const& k, ValueArgType const & v)
{
    typename MapType::iterator itrLB = m.lower_bound(k);
    if (itrLB != m.end() &&
       !(m.key_comp()(k,itrLB->first)) ){
        itrLB->second = v;
        return itrLB;
    }
    else{
        return m.insert(itrLB,make_pair(k,v));
    }
}
```

这个算法真的很有趣, 首先是算法的高效性, 其次是形式上的优美.

参考资料:
        Effective STL : item24
        The C++ Standard Libary

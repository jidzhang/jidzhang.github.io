---
title: "声明模板类为友元类"
date: 2015-05-10
draft: false
slug: "declear-friend-temple-class"
categories:
  - "cpp"
---

如题，要将模板类（template class）声明为友元类，需要注意两点：

（1）模板类本身的声明要足够早，我指的是在被声明为友元类之前就进行了模板类的声明。

比如：

	template<typename T>
	class AA
	{
	public:
	         ……
	private:
	        ……
	};

	class BB
	{
	public:
	      template<typename T> friend class AA;
	……
	 };

（2）友元类声明时的格式，应该是这样：

	template<typename T> friend class AA;

    AA是之前已经声明的模板类。

格式必须是这样，不然一定出错。

下面列出源代码，及源代码下载：

```cpp
//friend.h
#include <iostream>
using namespace std;
template<typename T>
class AA
{
    public:
        AA(T vl):val(vl){};
        void funcA();
    private:
        T val;
};
class BB
{
    public:
        BB(int d=0):tmp(d){};
        template<typename T>
            friend class AA;
    private:
        void funcB();
        int tmp;
};


template<typename T>
void AA<T>::funcA()
{
    BB bb(100);
    bb.funcB();
}

void BB::funcB()
{
    cout<< tmp <<endl;
}

//friendmain.cpp
#include "friend.h"
using namespace std;
int main()
{
    AA<int> aa(9);
    aa.funcA();
    return 0;
}

//欢迎交流
```

备注：

以上代码在VC8和GCC上均可以编译通过，但是在VC6上无法编译。这是VC6编译器自身的局限，而不是C++标准的原因。

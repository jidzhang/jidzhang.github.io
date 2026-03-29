---
title: "C/C++ 安全释放指针的方法"
date: 2015-03-10
draft: false
slug: "delete-pointer-in-c-safely"
categories:
  - "Code"
---

代码如下：

{% highlight cpp %}
#include <stdio.h>
#include <stdlib.h>

#define SAFE_FREE(ptr) { free(ptr); ptr = NULL;}
#define SAFE_DEL(ptr) { delete ptr; ptr = NULL;} 
#define SAFE_DELARR(ptr) { delete [] ptr; ptr = NULL;} 

int main(int argc, char **argv) 
{ 
	char* p = new char[100]; 
	SAFE_DELARR(p); 
	//system("pause"); 
	return 0;
}
{% endhighlight %}

实现安全释放指针的机制正是上面的三个宏，其本质就是在释放了指针后，一定要及时地把指针置为NULL。

注意：上面的代码包括了delete, delete[], 因此是段 C++代码。C的话只要第一个宏即可。

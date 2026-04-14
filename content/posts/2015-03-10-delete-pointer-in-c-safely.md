---
title: "C/C++ 安全释放指针的方法"
date: 2015-03-10
draft: false
slug: "delete-pointer-in-c-safely"
categories:
  - "Code"
---

代码如下：
```cpp
#include <stdio.h>
#include <stdlib.h>

#define SAFE_FREE(ptr) do { free(ptr); ptr = NULL; } while(0)
#define SAFE_DEL(ptr) do { delete ptr; ptr = NULL; } while(0)
#define SAFE_DELARR(ptr) do { delete [] ptr; ptr = NULL; } while(0) 

int main(int argc, char **argv) 
{ 
  char* p = new char[100]; 
  SAFE_DELARR(p); 
  //system("pause"); 
  return 0;
}
```

实现安全释放指针的机制正是上面的三个宏，其本质就是在释放了指针后，一定要及时地把指针置为NULL。宏体用 `do { ... } while(0)` 包裹，避免在 `if/else` 语句中使用时出现编译错误。

注意：上面的代码包括了delete, delete[], 因此是段 C++代码。C的话只要第一个宏即可。

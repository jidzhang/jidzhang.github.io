---
title: "关于'eh vector constructor/destructor iterator'的讨论及类的内存分布模型"
date: 2013-05-22
draft: false
slug: "eh-vector-constructor-iterator"
categories:
  - "Hack"
---

[原理分析]
	
在用IDA反汇编C++程序的时候，经常会看到这样的语句：`“call eh vector constructor iterator”`或`“eh vector destructor iterator”`。

第一反应：这是在调用某个std::vector<T>对象的构造函数或析构函数。但进一步的阅读发现跟std::vector<T>的实现对不上号，就是说程序中并没有声明std::vector对象。
>
	其实，这是调用new或delete导致的，即
		T * tt = new T[100]; 
		delete [] tt;
		其中，T是自定义类，并且自定义了构造函数或析构函数。

说到这里估计有人就会恍然大悟了。道理就是这么简单!

`“eh vector constructor/destructor iterator”`按字面上理解，就是：数组-构造-迭代器或数组-析构-迭代器。
它发生在创建一个自定义类的数组时，C++在分配内存后自动迭代调用类的构造函数以初始化每一个数组元素，或析构。

但，这里有一个问题：new的时候知道数组的大小，但是delete的时候没有指定数组个数，那么C++是怎么知道对多少个对象调用析构函数，并且释放多大内存呢？

C++程序员大概都有这样的疑问。

答案就是C++在创建数组的时候，在分配内存的头四个字节保存数组大小，从第4个字节开始才是数组内容，返回的时候也是返回的第4个字节之后的内存，所以导致大家感觉不到数组长度。

下面以一个简单的程序示范一下数组的创建过程、销毁过程、类的构造和析构、对象的内存分布模型，编译器是VC6.0，相信很多人都是在windows上搞反汇编的。

[示范程序]

	class demo_B {
	public:
		demo_B();
		~demo_B();
		void set_value(int a, float b);
	private:
		int a_;
		float f_;
	};
	demo_B::demo_B()
	{
		a_ = 1;
		f_ = -1.0;
	}
	demo_B::~demo_B()
	{
		a_ = -1;
		f_ = 0.0;
	}
	void demo_B::set_value(int a, float b)
	{
		a_ = a;
		f_ = b;
	}
	void test_foo(int n)
	{
		demo_B * buffer = new demo_B[n];
		delete [] buffer;
	}
	int main(int argc, char const *argv[])
	{
		test_foo(10);
		return 0;
	}


关键函数void test_foo(int n);用IDA反汇编如下：

	.text:00401020 ; void __cdecl sub_401020(int n)
	.text:00401020 sub_401020      proc near               ; CODE XREF: _main+2p
	.text:00401020
	.text:00401020 var_C           = dword ptr -0Ch
	.text:00401020 var_4           = dword ptr -4
	.text:00401020 n               = dword ptr  4
	.text:00401020
	.text:00401020                 mov     eax, large fs:0
	.text:00401026                 push    0FFFFFFFFh
	.text:00401028                 push    offset unknown_libname_10 ; Microsoft VisualC 2-10/net runtime
	.text:0040102D                 push    eax
	.text:0040102E                 mov     large fs:0, esp
	.text:00401035                 push    esi
	.text:00401036                 push    edi
	.text:00401037                 mov     edi, [esp+14h+n]
	.text:0040103B                 lea     eax, ds:4[edi*8]
	.text:00401042                 push    eax             ; unsigned int
	.text:00401043                 call    ??2@YAPAXI@Z    ; operator new(uint)
	.text:00401048                 add     esp, 4
	.text:0040104B                 mov     [esp+14h+n], eax
	.text:0040104F                 test    eax, eax
	.text:00401051                 mov     [esp+14h+var_4], 0
	.text:00401059                 jz      short loc_401077
	.text:0040105B                 push    offset sub_401010
	.text:00401060                 push    offset sub_401000
	.text:00401065                 lea     esi, [eax+4]
	.text:00401068                 push    edi
	.text:00401069                 push    8
	.text:0040106B                 push    esi
	.text:0040106C                 mov     [eax], edi
	.text:0040106E                 call    ??_L@YGXPAXIHP6EX0@Z1@Z ; `eh vector constructor iterator'(void *,uint,int,void (*)(void *),void (*)(void *))
	.text:00401073                 mov     eax, esi
	.text:00401075                 jmp     short loc_401079
	.text:00401077 ; ---------------------------------------------------------------------------
	.text:00401077
	.text:00401077 loc_401077:                             ; CODE XREF: sub_401020+39j
	.text:00401077                 xor     eax, eax
	.text:00401079
	.text:00401079 loc_401079:                             ; CODE XREF: sub_401020+55j
	.text:00401079                 test    eax, eax
	.text:0040107B                 mov     [esp+14h+var_4], 0FFFFFFFFh
	.text:00401083                 jz      short loc_4010A2
	.text:00401085                 mov     ecx, [eax-4]
	.text:00401088                 lea     esi, [eax-4]
	.text:0040108B                 push    offset sub_401010
	.text:00401090                 push    ecx
	.text:00401091                 push    8
	.text:00401093                 push    eax
	.text:00401094                 call    ??_M@YGXPAXIHP6EX0@Z@Z ; `eh vector destructor iterator'(void *,uint,int,void (*)(void *))
	.text:00401099                 push    esi             ; lpMem
	.text:0040109A                 call    sub_401120
	.text:0040109F                 add     esp, 4
	.text:004010A2
	.text:004010A2 loc_4010A2:                             ; CODE XREF: sub_401020+63j
	.text:004010A2                 mov     ecx, [esp+14h+var_C]
	.text:004010A6                 pop     edi
	.text:004010A7                 mov     large fs:0, ecx
	.text:004010AE                 pop     esi
	.text:004010AF                 add     esp, 0Ch
	.text:004010B2                 retn
	.text:004010B2 sub_401020      endp


按F5后，反编译如下：

	void __cdecl sub_401020(int n)
	{
	  void *v1;
	  int v2;
	  int v3;
	  void *v4;
	
	  v1 = operator new(8 * n + 4);                 // 为什么多分配了4个字节？
	  if ( v1 )
	  {
	    v2 = (int)((char *)v1 + 4);                 // 数组的实际内容从v1+4开始
	    *(_DWORD *)v1 = n;                          // 开头4字节用于记录数组元素个数
	    _eh_vector_constructor_iterator_((char *)v1 + 4, 8, n, sub_401000, sub_401010);
	    v3 = v2;
	  }
	  else
	  {
	    v3 = 0;                                     // C++自动检验是否正确的分配了需要的内存
	  }
	  if ( v3 )
	  {
	    v4 = (void *)(v3 - 4);                      // 向后4个字节即数组的元素个数，这个数据是被悄悄的记录了下来
	    _eh_vector_destructor_iterator_(v3, 8, *(_DWORD *)(v3 - 4), sub_401010);
	    sub_401120(v4);
	  }
	}


通过对上面的范例的分析，知道_eh_vector_constructor/destructor_iterator_的函数原型是这样：

	_eh_vector_constructor_iterator_(数组首地址,对象大小,数组个数,对象构造函数,对象析构函数);
	_eh_vector_destructor_iterator_(数组首地址,对象大小,*(_DWORD*)(数组首地址-4),对象析构函数);		//编译器知道到哪里找到数组个数


参考文献：**[Reversing Microsoft Visual C++ Part II: Classes,Methods and RTTI](www.openrce.org/articles/full_view/23)**
			


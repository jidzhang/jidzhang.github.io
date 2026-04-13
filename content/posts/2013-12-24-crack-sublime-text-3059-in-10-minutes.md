---
title: "10分钟破解Sublime Text 3 Build 3059/3061/3062"
date: 2013-12-24
draft: false
slug: "crack-sublime-text-3059-in-10-minutes"
categories:
  - "Hack"
---

更新
----
经过了差不多半年，Sublime Text的作者又开始更新了，可惜今年的更新频率远比不上前些年。难道作者不想玩了？    
2014年5月5号，作者在Dev频道上放出了更新Build 3062, 功能略有增加。    
下面是我修改过的sublime text, 包括了Windows 32/64, Linux 32/64 及 MacOS:

[**下载 Sublime Text Build 3062**](https://www.dropbox.com/sh/lqs2l4vocez6j8b/AABEZ35GQnj-mn-JbNAyHSppa)

[**下载 Sublime Text Build 3059**](https://www.dropbox.com/sh/n2xf7xvao1pya13/AADxxrbCekZzMU7zFV6sqng0a)

全部版本下载
----
3059,3061下载链接请转到：[**sublime text 完美破解**](http://apneng.net/2013/12/19/sublime-text-3059-windows-3264-cracker.html)

准备工具
----
* IDA ：强大的反汇编工具，目前最新的可用的版本是6.1， 可以到我的共享下载[IDA6.1](http://yun.baidu.com/share/link?shareid=1536167225&uk=2986591212)
* winhex ： 强大的十六进制编辑器，为了实现爆破必须有一个可用的十六进制编辑器。额，在这里用Vim确实不方便。可以到我的共享下载[winhex 17.2](http://yun.baidu.com/share/link?shareid=1008007072&uk=2986591212)
* sublime text原程序，到官网下载吧，我推荐下载Protable版，解压后即可用。对MacOS用户，道理是一样的，下载dmg镜像文件后直接解压即可得到名为Sublime Text的可执行程序

体验sublime text，发现突破点
----
下面我的说明都是以sublime text windows 32位的程序为例进行说明，在其他平台上的表现完全一致。    

打开sublime text程序，相信你已经把准备工作做好了。打开程序后新建文件随便写一些文字，然后狂按Ctrl+S进行保存，这时就会弹出注册对话框。如下图：
![This is an unregistered copy](https://dl.dropboxusercontent.com/u/6893139/images/sublime_text_3059_crack/2013-12-24_172542.png)    
看看这个对话框的标题：**This is an unregistered copy**，这就是我们的突破口。    

下面就开始搞。

分析程序，找到关键位置
----
- 用IDA加载sublime text可执行程序，你应该知道自己把可执行程序放哪里了。IDA分析程序要正经花些时间，等分析完了按一下Space键，就是按一下空格键。（感觉自己很麻婆）

- 在IDA里找菜单Search-->text(Alt+t), 打开搜索文本对话框，在搜索框内输入This is an，开始搜索，如下图：
![Text search](https://dl.dropboxusercontent.com/u/6893139/images/sublime_text_3059_crack/2013-12-24_174231.png)

- 现在程序跳到了.text:004C3FE2位置，这是一个很关键的函数，咱们对这个函数稍微分析一下，在IDA里面按F5，把汇编程序编译为C程序。

![unregistered copy](https://dl.dropboxusercontent.com/u/6893139/images/sublime_text_3059_crack/2013-12-24_174651.png)

![in c program](https://dl.dropboxusercontent.com/u/6893139/images/sublime_text_3059_crack/2013-12-24_174936.png)

看到最顶上的if语句了吗？if (byte_788D90) ....

这个byte_788D90在源代码里叫做g_valid_license，现在知道这个全局变量有多重要了吧。
下面就要对byte_788D90下手。

- 切到IDA View-A 汇编窗口, 往上滚动屏幕，找到byte_788D90，在byte_788D90上右键，然后选X。打开byte_788D90引用窗口。

![xref to byte_788D90](https://dl.dropboxusercontent.com/u/6893139/images/sublime_text_3059_crack/2013-12-24_175646.png)

看到标记为w的条目，现在我鼠标双击第6行，跳到.text:0049DE29

![g_valid_license](https://dl.dropboxusercontent.com/u/6893139/images/sublime_text_3059_crack/2013-12-24_183849.png)

这一段是什么意思呢？按F5，看到

![xx](https://dl.dropboxusercontent.com/u/6893139/images/sublime_text_3059_crack/2013-12-24_184038.png)

这里的意思是byte_788D90的值要根据函数sub_4C35AD而重置
`byte_788D90 = sub_4C35AD() != 0 ? byte_788D90 : 0`
我们要让这一句失效，就是把and byte_788D90, al改为nop nop nop nop nop nop。

好，现在我们记下**and byte_788D90, al**的位置：**0049DE29**

- 从上一步我们还看到函数sub_4C35AD也很重要，在源代码里，它的名字叫`check_license_file()`。

这一步我们就动手改函数sub_4C35AD。
双击call sub_4C35AD，跳到函数里面。然后滚动鼠标往下拉，很快看到大片的这种执行判断的代码：

![check_number](https://dl.dropboxusercontent.com/u/6893139/images/sublime_text_3059_crack/2013-12-24_184959.png)

相信你已经猜到这里是干什么了。继续往下拉，一直到这个函数的返回位置：
```asm
.text:004C3D12          mov     eax, edi
.text:004C3D14          call    sub_4F9CAE
.text:004C3D19          retn
```

其中，**mov eax, edi**就是设置函数的返回值。我们要把eax强制置为0。文章最后我会解释为什么设置为0，而不是1。
记下这里的位置**004C3D12**。
好了，现在找到要修改的位置，也知道改成什么样了，那现在就开始改可执行程序。

修改可执行程序
----
- 计算偏移值

刚才，我们记下了两个位置：0049DE29, 004C3D12
现在我们要这样计算代码在文件中的偏移
- 第一个偏移：49DE29 - 401000 + 400 = 9D229, 从这里开始的6个字符改为汇编nop，即90 90 90 90 90 90, **Linux的程序要改7个90**
- 第二个偏移：4C3D12 - 401000 + 400 = C3112, 从这里开始的2个字符改为汇编xor eax, eax 即31C0

- 用winhex打开sublime text可执行程序，按Alt+G，跳到上面的两个偏移位置 9D229和C3112，然后用键盘输入上面的十六进制代码
  - 在9D229的位置开始输入6个90
  - 在C3112的位置开始输入31C0
然后把文件另存，现在你另存的文件就是一个完美破解了的可执行程序，也许需要自己添加可执行权限。

![DONE](https://dl.dropboxusercontent.com/u/6893139/images/sublime_text_3059_crack/2013-12-24_192256.png)

最后
----
阅读这篇文章大概要花30分钟，但是动手操作起来实际用不了10分钟，以上的方法对于sublime text 3056和最新的3061完全适用。


补充
----
* sub_4C35AD为什么必需返回0 ？    
答：返回0可以保证无需输入注册码却能被sublime text识别为**Unlimited User License**， 无限制用户，岂不很爽。
* `byte_788D90 = sub_4C35AD() != 0 ? byte_788D90 : 0` 从这一步看sub_4C35AD返回1才对啊。    
答：对，这就是3059这一版的高明之处，sub_4C35AD要在注册的时候返回0才表明注册码有效，而在程序启动的时候它却应该返回1. 我也很无奈啊。



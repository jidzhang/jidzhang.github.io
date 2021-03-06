---
layout: post
title: "VIM查看文件编码、文件编码格式转换及文件名编码转换"
description: ""
category: vim
tags: []
---

如果你需要在Linux中操作windows下的文件，那么你可能会经常遇到文件编码转换的问题。

Windows中默认的文件格式是GBK(gb2312)，而Linux一般都是UTF-8。

下面介绍一下，在Linux中如何查看文件的编码及如何进行对文件进行编码转换。

## 查看文件编码

在Linux中查看文件编码可以通过以下几种方式：

1.在Vim中可以直接查看文件编码


	:set fileencoding


即可显示文件编码格式。
如果你只是想查看其它编码格式的文件或者想解决用Vim查看文件乱码的问题，那么你可以在
~/.vimrc 文件中添加以下内容：

	set encoding=utf-8
	set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1

这样，就可以让vim自动识别文件编码（可以自动识别UTF-8或者GBK编码的文件），其实就是依照fileencodings提供的编码列表尝试，如果没有找到合适的编码，就用latin-1(ASCII)编码打开。


## 文件编码转换

1. 在Vim中直接进行转换文件编码,比如将一个文件转换成utf-8格式

	```
	:set fileencoding=utf-8
	```

2. iconv 转换，iconv的命令格式如下：

	```
	iconv -f encoding -t encoding inputfile
	```

	比如将一个UTF-8 编码的文件转换成GBK编码

	```
	iconv -f GBK -t UTF-8 file1 -o file2
	```

## 文件名编码转换:

从Linux往 windows拷贝文件或者从windows往Linux拷贝文件，有时会出现中文文件名乱码的情况，出现这种问题的原因是因为，windows的文件名中文编码默认为GBK,而Linux中默认文件名编码为UTF8,由于编码不一致，所以导致了文件名乱码的问题，解决这个问题需要对文件名进行转码。

在Linux中专门提供了一种工具convmv进行文件名编码的转换，可以将文件名从GBK转换成UTF-8编码,或者从UTF-8转换到GBK。

首先看一下你的系统上是否安装了convmv,如果没安装的话用:

	yum -y install convmv

安装。

下面看一下convmv的具体用法：

	convmv -f 源编码 -t 新编码 [选项] 文件名

常用参数：

	-r 递归处理子文件夹
	--notest 真正进行操作，请注意在默认情况下是不对文件进行真实操作的，而只是试验。
	--list 显示所有支持的编码
	--unescap 可以做一下转义，比如把%20变成空格

比如我们有一个utf8编码的文件名，转换成GBK编码，命令如下：

	convmv -f UTF-8 -t GBK --notest utf8编码的文件名

这样转换以后"utf8编码的文件名"会被转换成GBK编码（只是文件名编码的转换，文件内容不会发生变化）

## Vim 编码方式的设置

和所有的流行文本编辑器一样，Vim 可以很好的编辑各种字符编码的文件，这当然包括UCS-2、UTF-8 等流行的 Unicode 编码方式。然而不幸的是，和很多来自 Linux 世界的软件一样，这需要你自己动手设置。

Vim 有四个跟字符编码方式有关的选项，encoding、fileencoding、fileencodings、termencoding (这些选项可能的取值请参考 Vim 在线帮助 :help encoding-names)，它们的意义如下:

* encoding: Vim 内部使用的字符编码方式，包括 Vim 的 buffer (缓冲区)、菜单文本、消息文本等。默认是根据你的locale选择.用户手册上建议只在 .vimrc 中改变它的值，事实上似乎也只有在.vimrc 中改变它的值才有意义。你可以用另外一种编码来编辑和保存文件，如你的vim的encoding为utf-8,所编辑的文件采用cp936编码,vim会自动将读入的文件转成utf-8(vim的能读懂的方式），而当你写入文件时,又会自动转回成cp936（文件的保存编码).

* fileencoding: Vim 中当前编辑的文件的字符编码方式，Vim 保存文件时也会将文件保存为这种字符编码方式 (不管是否新文件都如此)。

* fileencodings: Vim自动探测fileencoding的顺序列表， 启动时会按照它所列出的字符编码方式逐一探测即将打开的文件的字符编码方式，并且将 fileencoding 设置为最终探测到的字符编码方式。因此最好将Unicode 编码方式放到这个列表的最前面，将拉丁语系编码方式 latin1 放到最后面。

* termencoding: Vim 所工作的终端 (或者 Windows 的 Console 窗口) 的字符编码方式。如果vim所在的term与vim编码相同，则无需设置。如其不然，你可以用vim的termencoding选项将自动转换成term的编码.这个选项在 Windows 下对我们常用的 GUI 模式的 gVim 无效，而对 Console 模式的Vim 而言就是 Windows 控制台的代码页，并且通常我们不需要改变它。

好了，解释完了这一堆容易让新手犯糊涂的参数，我们来看看 Vim 的多字符编码方式支持是如何工作的。

1. Vim 启动，根据 .vimrc 中设置的 encoding 的值来设置 buffer、菜单文本、消息文的字符编码方式。

2. 读取需要编辑的文件，根据 fileencodings 中列出的字符编码方式逐一探测该文件编码方式。并设置 fileencoding 为探测到的，看起来是正确的 (注1) 字符编码方式。

3. 对比 fileencoding 和 encoding 的值，若不同则调用 iconv 将文件内容转换为encoding 所描述的字符编码方式，并且把转换后的内容放到为此文件开辟的 buffer 里，此时我们就可以开始编辑这个文件了。注意，完成这一步动作需要调用外部的 iconv.dll(注2)，你需要保证这个文件存在于 $VIMRUNTIME 或者其他列在 PATH 环境变量中的目录里。

4. 编辑完成后保存文件时，再次对比 fileencoding 和 encoding 的值。若不同，再次调用 iconv 将即将保存的 buffer 中的文本转换为 fileencoding 所描述的字符编码方式，并保存到指定的文件中。同样，这需要调用 iconv.dll由于 Unicode 能够包含几乎所有的语言的字符，而且 Unicode 的 UTF-8 编码方式又是非常具有性价比的编码方式 (空间消耗比 UCS-2 小)，因此建议 encoding 的值设置为utf-8。这么做的另一个理由是 encoding 设置为 utf-8 时，Vim 自动探测文件的编码方式会更准确 (或许这个理由才是主要的 ;)。我们在中文 Windows 里编辑的文件，为了兼顾与其他软件的兼容性，文件编码还是设置为 GB2312/GBK 比较合适，因此 fileencoding 建议设置为 chinese (chinese 是个别名，在 Unix 里表示 gb2312，在 Windows 里表示cp936，也就是 GBK 的代码页)。

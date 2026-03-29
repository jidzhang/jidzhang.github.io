---
title: "build vim on Linux"
date: 2013-11-02
draft: false
slug: "build-vim-on-linux"
categories:
  - "Geek"
---

以下是我的配置和编译过程，主要是比默认的多开启了ruby,lua,python2,perl,tcl，这样做的目的是可以顺利的保证Vim插件（比如YCM和NeoComplete）的使用：

    (1) ./configure --with-features=huge --enable-rubyinterp --enable-luainterp=dynamic --enable-pythoninterp --with-python-config-dir=/usr/lib/python2.7/config --enable-perlinterp --enable-cscope --enable-tclinterp --enable-multibyte --enable-clipboard --prefix=/usr
    (2)  make VIMRUNTIMEDIR=/usr/share/vim/vim74
    (3)  sudo make install

以上的配置开启了ruby,lua,python2,perl,tcl. 而Python3有意没开启，因为同时开启python2和python3的话会导致YouCompleteMe不能用。    
这样的配置可以保证[YouCompleteMe](https://github.com/Valloric/YouCompleteMe)和[NeoComplete](https://github.com/shougo/neocomplete.vim)都可以正常使用。

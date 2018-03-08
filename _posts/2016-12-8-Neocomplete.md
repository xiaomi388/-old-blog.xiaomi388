---
layout: post
title: 树莓派下为Vim配置Neocomplete
tags: [Code]
---
由于 Neocomplete 依赖于 lua ，而用 apt 安装的 vim 是不带 lua 的，所以首先得重新手动编译带 lua 的 vim，具体教程可参考[http://www.cnblogs.com/spch2008/p/4593370.html][1]，这篇教程说得很好，每一个步骤都缺一不可...

其次是安装 Neocomplete ，直接在 Github 上就可找到源码和安装教程。[https://github.com/Shougo/neocomplete.vim][2]



[1]:	http://www.cnblogs.com/spch2008/p/4593370.html
[2]:	https://github.com/Shougo/neocomplete.vim
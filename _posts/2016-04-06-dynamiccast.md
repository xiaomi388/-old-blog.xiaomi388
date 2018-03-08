---
layout: post
title: C++ 中的 dynamic_cast 与 static_cast 详解
tags: [Code]
---

主要格式：

    dynamic_cast <type-id> (expression)

该运算符把expression转换成type-id类型的对象。Type-id 必须是类的指针、类的引用或者void*；若 Type-id 是类的指针或引用，则 expression 也必须是指针或者引用。

使用dynamic_cast的好处：简单来说，如果可以成功转型，则返回适当转型过的指针；如果不可成功转型，则返回空指针。而 static_cast 若不能成功 转型，则会发生编译错误。

dynamic_cast主要用于类层次间的上行转换和下行转换，还可以用于类之间的交叉转换。

在类层次间进行上行转换时，dynamic_cast和static_cast的效果是一样的；

在进行下行转换时，dynamic_cast具有类型检查的功能，比static_cast更安全。







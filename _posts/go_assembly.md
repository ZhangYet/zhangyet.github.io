---
title: "go assembly 简介"
tags: ["go", "assembly", "programming"]
date: 2019-05-02 18:08:13
excerpt: 简介 go assembly 并用它来探索一下 go 一些内部机制
---

## [官方文档](https://golang.org/doc/asm) ##

`go` 的汇编与 Plan 9 的汇编其实有很大区别，比如常量计算的优先级，`go` 是 `(3&1)<<2` 而不是 `3&(1<<2)` [^1]，所有常量都是64位无符号整数[^2]。

有些符号，比如 `R1` 或者 `LR` 会随架构而改变。但是有四个与定义的符号是不变的。

1. FP: frame 指针，参数或者局部变量；
2. PC: program 计数器，jump and branches;
3. SB: static base pointer，全局变量；
4. SP：栈指针，指向栈顶；

其余所有用户写的符号，都会用相对 FP 和 SB 的偏移量来定义。SB 可以认为是内存空间的起点。

但不得不说，这篇官方文档实在太枯燥了。

## [Go Assembly 示例](https://colobu.com/goasm) ##

这个工具就友好得多了。但是这些例子和新版本 `go compile -S` 出来的汇编都有一点区别，所以需要一点功夫去对应过来。

## [《Go语言高级编程》第3章](https://chai2010.cn/advanced-go-programming-book/ch3-asm/readme.html) ##



[^1]: 所以你们日常加括号会死么，会么会么！

[^2]: 这就是新时代的好处啊。

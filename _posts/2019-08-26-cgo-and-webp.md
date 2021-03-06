---
title: "用 cgo 封装 libwebp"
tags: ["go", "c"]
date: 2019-08-26 16:00:54
layout: post
excerpt: 周末在家用 cgo 封装了一下 libwebp，用它写了个小程序将 webp 转换成 png。
categories: programming
comments: true
---

## 缘起 ##

我现在的头像来自[这个相册](https://www.douban.com/photos/album/49597443/)，但是从网上复制下来的格式是 [webp](https://developers.google.com/speed/webp/)，没办法直接用来做头像。于是我顺便看了一下，有没有工具可以转格式。

当然 google 已经提供了相应的命令行工具，但是当我看到它只有 C 实现的 library 的时候，我不由得动了用 cgo 将 libwebp 封装成 go library 的想法——部分原因是我刚好读了[《Go 语言高级编程》](https://book.douban.com/subject/34442131/)[^1]。

## 过程 ##

主要的准备工作：

1. 看 libwebp 的文档；
2. 看 go image/png 的文档，了解如何用 go 生成一张 png 图片；
3. 看如何用 cgo 调用一个 c library；
4. 初始化项目，将 libwebp 的代码 init 成项目的 submodule，失败[^2]，下载了代码；
5. 尝试将 libwebp 编译成动态库，失败一下 c 编译的各项技术，中途绝望、沮丧若干次；
6. 放弃使用动态库，下载编译好的静态库；
7. 重拾遗忘已久的 c 语言技能，写了一个 `WebPGetInfo` 的 wrapper 函数（这样 go 语言调用的时候入参可以方便一点），并编译通过，运行成功[^3]；
8. 在 go 中通过 cgo 调用 c 库成功，中途经历若干次 linker 找不到函数等 c 语言编译问题/文件路径没有写对问题；
9. 封装 `WebPDecodeRGBA` 函数；
10. 在 go 中调用 `WebPDecodeRGBA` 函数，解决「如何在 go 中访问 c 指针指向的数据」[^4]
11. 输出的图像有问题，各种 debug，手段包括但不限于：重新读文档，确认自己没有用错接口；换一张图片作为测试，确认函数真的有成功读入和写入；在床上打滚；
12. 最后发现是算 index 的时候出了问题：c 返回的数据是一个一维的 array，每四个元素代表一个像素点；
13. 转换之后跟在线转换工具对比，发现转换成功（质量一样差）[^5]
14. 觉得太麻烦了，不再尝试封装成库了。

## 总结 ##

其实 api 封装工程师，也不是容易做的呀。代码还没有整理好，等我有空再发出来。

## 脚注 ##

[^1]: 对这本书，我的评价是: **对本书的批评主要是两点：1. 跟 go 没有直接关系的内容略多；2. 每个主题讲得都不够好，往往在正要讲到痛处的时候宕开一笔，转向另一个方向。每章基本都是独立的，没有构成体系。另外有排版错误（如213页）。优点在于选材，cgo （周末就在家捣鼓这个了，好难用）和 go 汇编有助于加深对 go 的理解（然而本书讲得不咋的），第四章和第五章讲 rpc 和 web 框架的部分是全书写得最好的部分。——当然，以上评价都有可能因为我水平不够而有失公允**。

[^2]: 吔屎啦 GFW。

[^3]: 同时还解决了「如何用 c 语言读入一个 webp 文件」的问题。

[^4]: 其实就是在 c 里面封装了一个函数，接受指针和 index，返回对应 index 的值。

[^5]: 但貌似 webp 可以把图片放大。

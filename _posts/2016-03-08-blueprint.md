---
layout: post
title: "关于Blueprint的一些想法"
comments: true
---
我之前不了解`Blueprint`的用处。

直到最近我用`flask`重构`prometheus`，我遇到一个问题：我想在一个module里面定义各种handler，然后在main里面创建app，指定url。问题就是如何将参数传入。在不了解`Blueprint`的时候，我只能在各个handler那个留出参数，然后在app里面把request中的参数传进去，非常累赘。现在我终于可以在不同的模块定义不同的handler，在定义这些handler的同时为它们分配url。

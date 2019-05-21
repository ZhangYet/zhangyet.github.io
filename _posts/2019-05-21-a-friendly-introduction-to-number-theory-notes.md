---
title: "《数论概论》笔记"
tags: ["math"]
date: 2019-05-21 12:33:01
layout: post
excerpt: 《数论概论》的读书笔记。
categories: math
comments: true
---

本文是[《数论概论》](https://book.douban.com/subject/1884445/)的读书笔记[^1]

## 线性同余定理 ##

线性同余定理解决了形如 $ax \equiv c \pmod p$ 的同余方程的解的问题。这个定理不但判断了方程是否有解，还给出了求解的方法。这个定理的证明关键点在于将方程 $ax \equiv c \pmod p$ 变形成 $ax - my = c$, 由第六章定理6.1可知，$ax-my$ 是 $g = \gcd(a, m)$ 的倍数。由此可知，如果 $c$ 与 $g$ 互质，则线性方程无解，否则同余方程 $ax \equiv c \pmod p$ 的解可以由 $ax - my = c$ 的解 $u_0$ 和 $g$ 表出。

[^1]: 早这么勤奋当年读书就没有那么痛苦了。

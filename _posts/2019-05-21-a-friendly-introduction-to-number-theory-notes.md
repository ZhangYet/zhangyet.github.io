---
title: "《数论概论》笔记"
tags: ["math"]
date: 2019-05-21 12:33:01
layout: post
excerpt: 《数论概论》的读书笔记。不得不说，这本书就好像评书一样，到处留扣子。
categories: math
comments: true
---

本文是[《数论概论》](https://book.douban.com/subject/1884445/)的读书笔记[^1]

## 线性同余定理 ##

线性同余定理解决了形如 $ax \equiv c \pmod p$ 的同余方程的解的问题。这个定理不但判断了方程是否有解，还给出了求解的方法。这个定理的证明关键点在于将方程 $ax \equiv c \pmod p$ 变形成 $ax - my = c$, 由第六章定理6.1可知，$ax-my$ 是 $g = \gcd(a, m)$ 的倍数。由此可知，如果 $c$ 与 $g$ 互质，则线性方程无解，否则同余方程 $ax \equiv c \pmod p$ 的解可以由 $ax - my = c$ 的解 $u_0$ 和 $g$ 表出。

## 费马小定理 ##

令 $p$ 是一个质数，$a$ 与 $p$ 互质，则有

$$ a^{p-1} \equiv 1 \pmod p $$

定理的证明非常巧妙，通过证明 $a,2a,...,(p-1)a \pmod p$ 的余数就是 $1,2,...,(p-1)$ (实际上只需要证明刚好有 $p-1$ 个不同的余数即可)。

费马小定理可以用于寻找大数的因数分解。

## 欧拉公式 ##

从费马小定理出发，我们希望将形如 $a^x \equiv 1 \pmod m$ 推广到 m 是一般自然数的情况（或者说，推广到 $m$ 是合数的情况）。首先我们看到若 $\gcd(a, m) > 1$ 这个方程必然是无解的[^2]。

所以我们定义了 欧拉 $\Phi$ 函数：

$$\Phi(m)=\sharp\{a: 1 \leq a \leq m \wedge \gcd(a, m) = 1\}$$

那么，当 $\gcd(a, m)=1$ 的情况下，我们有：

$$a^{\Phi(m)} \equiv 1 \pmod p$$

证明方式跟费马小定理差不多。

## 中国剩余定理 ##

欧拉公式当然好，但是如果无法计算 $\Phi$ 函数的值也就没有价值了。

于是我们有 $\Phi$ 函数公式：

1. 若 $p$ 是质数，且 $k \leq 1$，那么我们有 $$\Phi(p^k)=p^k-p^{k-1}$$ 。

2. 若 $\gcd(m, n)=1$，那么我们有 $\Phi(mn)=\Phi(m)\Phi(n)$

1 的证明很简单，因为 $p^k$ 的全部因子就是 $p,2p,3p,...,(p^{k-1}-1)p,p^k$，总共 $p^{k-1}$ 个。

2 告诉我们，如果我们可以找到 $m$ 的因数分解，那么我们就可以计算 $\Phi(m)$[^3]。

然后我们证明 2。

虽然书里用了特写的 **COUNTING** 来说明证明的手法，但其实真正证明方法是构造双射。

**证明**: 

令集合 

$$A = \{a: 1 \leq a \leq mn \wedge \gcd(a, mn)=1\}$$, 

$$B = \{(b, c): 1 \leq b \leq m \wedge \gcd(b, m) = 1 \wedge  1 \leq c \leq n \wedge gcd(c, n) = 1\}$$，
	
只需要证明存在 $A$  到 $B$ 的双射即可。

首先证明单射，令 $a_1, a_2 \in A$, 假设 $a_1 \equiv a_2 \pmod m$ 且 $a_1 \equiv a_2 \pmod n$，往征 $a_1 \equiv a_2 \pmod mn$。 

因为 $a_1 - a_2$ 能同时整除 $m$ 和 $n$，又因为 $\gcd(m,n)=1$ 所以 $a_1 - a_2$ 可以整除 $mn$，所以 $a_1 \equiv a_2 \pmod {mn}$。

下面证明满射，即证明方程组

$$
\begin{cases}
a \equiv b \pmod m \\
a \equiv c \pmod n
\end{cases}
$$

有解，这就是**中国剩余定理**的内容了

中国剩余定理的证明其实就是把 $a$ 表示成 $mx+b$，再代入 $a \equiv c \pmod n$ 中，这样我们可以得到 $mx \equiv c-b \pmod n$，由**线性同余定理**可知有唯一解。

## 素数 ##

> 2 is the oddest prime![^4]

本章主要的定理是 [Dirichlet's theorem on arithmetic progressions](https://en.wikipedia.org/wiki/Dirichlet%27s_theorem_on_arithmetic_progressions), 它给出了这样一个结果：若 $\gcd(a, m) = 1$, 则 $ p \equiv a \pmod m $ 有无穷的整数解。这个定理和黎曼猜想有关，可以参阅[《素数之恋》](https://book.douban.com/subject/3541506/)。

## 素数的数量 ##

素数的数量是无限的，对它数量的一个估计由 **the prime number theorem** 给出，这个定理说明 $\pi(x) \sim \frac{x}{ln(x)}$, $\pi(x)$ 表示小于 $x$ 的素数数目。这里提及到了分析数论。 

## 梅森素数 ##

假如我们想找形如 $a^x-1$ 的素数，那么 $a$ 只能等于 2，否则它一定有因子 $a-1>1$，$x$ 必须是素数，否则，令 $x = m \cdot k$ 则必有因子 $a^m-1>1$, 形如 $2^p-1$ 的素数就是梅森素数。


## 脚注 ##

[^1]: 早这么勤奋当年读书就没有那么痛苦了。

[^2]: 否则 $a^x = k \cdot m + 1$，但是 $a^x = l \cdot m$ 与 $k \cdot m + 1$ 互质。

[^3]: 也就是说，我们将一个困难的问题转化成另一个困难的问题了。

[^4]: 作者居然特意在这里讲了个冷笑话，并且在脚注里面标注了“这个笑话太烂了”。

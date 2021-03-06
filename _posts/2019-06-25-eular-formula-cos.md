---
title: "用欧拉公式推导三角函数和角公式"
tags: ["math"]
date: 2019-06-25 21:58:36
layout: post
excerpt: 我已经完全忘记如何用初等方法推导三角公式了，幸好我大学的学费没有白交，我忽然想起了用欧拉公式推导和角公式的方法
categories: math
comments: true
---

昨天我的前同事海大爷忽然问我关于三角函数公式的事情，高考完了这么久之后，我发现我完全忘记了那些和角公式——不但如此，我还忘记了怎样推导的！只能悻悻地在知乎找了个[帖子](https://zhuanlan.zhihu.com/p/39404639)给他。

今天忽然想起，其实我可以用[欧拉公式](https://zh.wikipedia.org/wiki/%E6%AC%A7%E6%8B%89%E5%85%AC%E5%BC%8F)推导一次，推导过程如下：

## 推导 ##

因为

$$ e^{ix} = \cos{x} + i\sin{x}$$

所以[^1]:

$$ e^{-ix} = \cos{x} - i\sin{x} $$

两式相加，正负抵消，我们有

$$ \cos{x} = \frac{e^{ix}+e^{-ix}}{2} $$

然后我们将 $x=A+B$ 代入上面这条等式中，注意到 

$$e^{i(A+B)}=e^{iA}e^{iB}=(\cos{A}+i\sin{A})(\cos{B}+i\sin{B})$$

所以，我们可以知道

$$ 2 \cos{A+B} = (\cos{A}+i\sin{A})(\cos{B}+i\sin{B}) + (\cos{A}-i\sin{A})(\cos{B}-i\sin{B}) $$

最后，做乘法分配律，同类项消除，我们就可以得到余弦的和角公式了：

$$ \cos{(A+B)} = \cos{A}\cos{B} - \sin{A}\sin{B} $$

至于其余三角函数的公式，留作习题[^2]

## 感想 ##

忽然想起这种方法，就好像在一件很久没有穿过的衣服里翻出了10块钱——虽然百无一用，却略带慰藉。

当然，事后我再翻欧拉公式的 wiki 时，我发现 wiki 里面就有这条公式的推导。真是浪费时间。


## 附注 ##

[^1]: 注意到 $\sin$ 是奇函数而 $$cos$$ 是偶函数

[^2]: 打 $\LaTeX$ 公式好累的。

---
layout: post
title: "漏译补充"
comments: true
---
```
> plot(x, y, pch=21, bg="gray", ylim=c(-3, 3), asp=1)
> adjy <- spread.labs(y, strheight("10", cex=1.5))
> text(-0.5, adjy, labels=1:10, pos=2)
> segments(-0.5, adjy, x, y)

```
另一种选择就是maptools包的pointLabel()函数。
```
>libarary(maptools)
```
这个函数会考虑标签相对数据点的八个位置（下方、左下、左边和左上等）并尝试为所有标签选择最优的位置。这与thigmophobe.labels()相似，但算法更复杂，标签画得离数据点更近。下面得代码描述了这个函数的用法（见图11.5）。

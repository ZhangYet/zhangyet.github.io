---
title: "TinyLFU 一种缓存策略"
tags: ["arxiv", "cache", "papers"]
date: 2019/03/25 10:01:03
excerpt: TinyLFU 使用 counting bloom filter 估计 key 的分布，从而优化缓存的效率——压缩存储空间和提高命中率。
---

# TinyLFU 一种缓存策略

读 [The State of Caching in Go](https://blog.dgraph.io/post/caching-in-go/) 的时候知道了 [caffeine](https://github.com/ben-manes/caffeine)， 知道它是基于 TinyLFU 实现的，所以找了[文章](https://arxiv.org/pdf/1512.00727.pdf)来看看。

## 导论

缓存的底层原理是 [locality 假设](https://en.wikipedia.org/wiki/Central_tendency)（就像统计学家通常假设正态分布一样）。而这种 locality 是可以通过频次统计来估计出来的。

命中率最高的策略就是 LFU，但是 LFU 有两个缺点：1. 需要维护比较大的元数据（比如说 key 的频数统计，缓存根据元数据来决定那些 key 需要清退）；2. 频数变化可能会很快。

与 LFU 相对的是 LRU， LRU 保证最新的 key 在前面，淘汰最旧的 key。LRU 效率会更高，但是需要更大的缓存空间才能达到 LFU 的命中率。(但是我自己做了个实验，发现并非如此：在 zipf 分布的情况下，LFU 的命中率比 LRU 的命中率高不了多少，但是耗时明显比 LRU 高，在均匀分布的条件下，两者的表现都很差， 测试用的代码在[这里](https://github.com/ZhangYet/ahotori/tree/master/Demos/CacheTest)， 当然我这里的结论可能不正确，因为我的测试可能不够全面，无论如何，我觉得在大部分场景下， LRU 都比 LFU 有优势——命中率差不多，速度快，实现更简单)。

`TinyLFU` 的改进之处在于：

1. 对某个 key 是否放入缓存，有一个准入策略，需要判断把 key 放进缓存有利才会放；
2. 更紧凑的数据结构；

## 相关工作

这个章节提到各种 LFU 的改进方案。另外提及了一些近似计数方案(approximate counting architecture)：

* `TinyLFU` 不考虑抽样方法，因为抽样方法需要 keys 有 explicit representation 这个增加成本。
* [Counter Braids](https://web.stanford.edu/~montanar/RESEARCH/FILEPAP/sigmetrics08_final.pdf) 可以减少 meta data 的尺寸，但是解码麻烦，所以也不考虑。
* 综合比较了几种 multi hash sketches，最后选择了 [Counting bloom filter](https://cloud.tencent.com/developer/article/1136056) 加上 [minimal increment scheme](http://www.cs.technion.ac.il/~gilga/CS-2014-04.pdf)，这个方案。

（以上文献我都没读过——一篇文献带出无数文献，要真读了我得死去啊，这一方面也反映了 IT 从业人员的艰辛。）

## TinyLFU 架构

总的来说就是每次需要淘汰 key 的时候，就是拿出新的 key 取它在计数器里面的值，跟需要淘汰的 key 的值比较，如果新的 key 的值更大，那么就淘汰旧的（但我不明白怎能取到新的 key 的计数，就算可以吧，这个新的 key 的计数为什么可以代表它的频数）。

### 近似计数

这里采用了 minimal increment CBF。支持两种方法：

1. `Estimate`: 计算 k 个不同的 hash 值，以 hash 值为索引，找到对应的计数器，然后返回最小的值。
2. `Add`: 计算 k 个不同的 hash 值，以此为索引，增加值最小的计数器的值（如果值最小的计数器有多个，则增加多个计数器的值）。

因为采用了 minimal increment 策略，所以不支持减少计数，但是有利于统计高频的 key，详细的分析可以看[这篇文献](http://www.cs.technion.ac.il/~gilga/CS-2014-04.pdf)（然而我还没有看）。

### 更新机制(freshness mechanism)

3.3.2 两条引理就是说“频数接近期望”，这都能写一节，明显是欺负 CS 的人概率学读得少啊。

但是这一节（3.3）我没有怎么读懂，为什么要在达到一定的 sample size 之后除以2，看起来挺莫名其妙的。

## 总结

从总体思路上，`TinyLRU` 就是换用了一种更高效的方法（且在空间和时间上平衡）去估算 key 的分布。这让我想起2017年，用机器学习去估 key 分布从而优化数据库索引的[这篇文章](https://arxiv.org/abs/1712.01208)。有可能这是一种新套路：通过估变量分布来优化性能，或者我们应该搞一个公共组件专门来做这种事情。

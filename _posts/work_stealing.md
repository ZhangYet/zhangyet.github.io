---
title: "work strealing 算法"
tags: ["algorithm", "golang", "programming", "papers"]
date: 2019/04/08 03:04:51
excerpt: Scheduling Multithreaded Computations by Work Stealing 的阅读笔记
---

本文是 [Scheduling Multithreaded Computations by Work Stealing](http://supertech.csail.mit.edu/papers/steal.pdf) 的笔记，该论文提出了基于 work stealing 思想的线程分配算法。

## work stealing 的定义

* **Work-sharing** 产生新线程的时候，进程将已有的线程分配出去。
* **Work-stealing** 利用率低的进程从别的进程那里“偷”线程。

> 吐槽，资本主义不但压榨人，也压榨进程。

## 多线程模型

将多线程计算抽象成一个 [DAG](https://en.wikipedia.org/wiki/Directed_acyclic_graph)，每个指令是一个节点，指令的依赖关系构成边，这种边被称为 *continue* 边。线程会 spawn 出新的线程，这在图中增加了新类型的边，我们称这种边为 *spawn* 边。没有 *spawn* 边出去的线程被称为叶子线程。不同的指令之间可能会有数据的依赖，若指令 b 依赖指令 a 的数据，则 a 到 b 存在一条 *join* 边。

一个 **fully strict** 的计算模型就是所有 *join* 边都是从子线程指向父线程（不会从父线程到子线程，也不会从子线程到祖父线程）。

> 子线程、父线程和祖父线程这种说法比较不女权，但是也没有更好的术语用。

## the busy-leaves property

The **busy-leaves property**：运行中，every leaf has a  processor working on it.

> 不知道怎样翻译才好。

The Busy-Leaves Algorithm （后面简称 BL 算法）有几条规则：

1. 如果线程 a spawn 了线程 b，processor 将 a 放回线程池，开始执行线程 b 的指令。
2. 如果线程 a stalls （等待它依赖的线程的数据），processor 将 a 放回线程池，然后开始闲置。
3. 如果子线程 a 死了，检查它的父线程 b 是否有其他子线程，如果 b 既没有活着的子线程也没有别的 proccessor working on it，那么 proccessor 执行 b，若否， proccessor 闲置。

strict 的计算模型可以保持 the busy-leaves property。 证明就省下了。但是 BL 算法不够高效。

## 随机化 work-stealing 算法

首先，每个 proccessor 都会维护一个 dequeue。对 proccessor 来说，这个 dequeue 和 stack 一样，是后进先出的，但是可以在头部移走线程（这样就可以”偷“线程了）。

WS 算法的规则如下：

1. 如果线程 a spawn 线程 b，把线程 a 放入 ready queue 的底部， proccessor 开始处理 b 的指令。
2. 如果线程 a stalls，proccessor 检查 ready queue，如果里面有线程，那么取出线程开始处理。如果没有，开始 work stealing：从随机选择的 proccessor 的 queue 的顶部取线程。
3. 如果线程 a 死了，按照规则2的方案走。
4. 如果线程 a enable 了线程 b，线程 b 被放入线程 a 的 proccessor 的 queue 里面。

定理和证明略。

剩下的部分都是算法分析和定理证明。略过不看。
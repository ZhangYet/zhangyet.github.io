---
title: "残酷面试的复习纲要"
tags: ["programming"]
date: 2019-12-28 18:02:53
layout: post
excerpt: 帮我同事整理面试相关的知识，整着整着发现我也需要，于是顺便在这里记录一下。
categories: programming
comments: true
---

## HTTP(原理 请求头 请求体) ##

参考文献：
[MDN Http 文档](https://developer.mozilla.org/en-US/docs/Web/HTTP)

重点：
1. [Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
2. [headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
3. [methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
4. [http messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)

需要浏览的点：

[status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)


难点：[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

## HTTPS ##

[《图解HTTP》](https://kingyinliang.github.io/PDF/%E5%9B%BE%E8%A7%A3HTTP+%E5%BD%A9%E8%89%B2%E7%89%88.pdf) 第七章

## TCP ##

1. 三次握手，[为什么 TCP 建立连接需要三次握手](https://draveness.me/whys-the-design-tcp-three-way-handshake)
2. 为什么需要四次挥手？

参考文献：[《图解 TCP/IP》](https://github.com/dolotech/ebook/blob/master/%E3%80%8A%E5%9B%BE%E8%A7%A3TCP%20IP(%E7%AC%AC5%E7%89%88)%E3%80%8B.((%E6%97%A5)%E7%AB%B9%E4%B8%8B%E9%9A%86%E5%8F%B2).%5BPDF%5D.%26ckook.pdf) 6.4 

## DNS ##

其实 DNS 没啥好讲的，过一下 《图解 TCP/IP》 5.2 即可

## UDP ##

看《图解 TCP/IP》 6.3


## MYSQL 面试题与 SQL 常用 ##

增删改查，备份，索引，主从数据库，inner/left/outer/right on 和 where 区别谁先谁后，分库分表

## Git 熟练消化理解 ##

1. fetch 与 pull 的区别；
2. 如何产生/解决 conflict；
3. rebase；
4. cherry-pick
5. log 以及 log search

## python 基本语法 ##

接着看完，能独立完成一个爬虫程序（至少能完全看懂）

## Redis ##

过一遍： [commands](https://redis.io/commands)，起码知道 redis 有什么功能。

难点：

1. https://redis.io/topics/distlock
2. https://redis.io/topics/pubsub


## RPC ##

<https://zh.wikipedia.org/wiki/%E9%81%A0%E7%A8%8B%E9%81%8E%E7%A8%8B%E8%AA%BF%E7%94%A8>

还是得看看这个啊：https://grpc.io/docs/guides/

## MQ 相关文章阅读 ##

[什么是 MQ](https://aws.amazon.com/cn/message-queue/)

[kafka](https://kafka.apache.org/documentation/) 需要了解的东西太多，你看不过来的。

## Zookeeper ##

## Docker ##

## ORM ##

看一下这个 [stackoverflow 回答](https://stackoverflow.com/questions/1279613/what-is-an-orm-how-does-it-work-and-how-should-i-use-one)就好

## Linux ##

第六章（文件处理）/第十二章 （进程相关）

Shell Script

## 基础算法题 50 题 python 语言 ##

1. 堆
2. 栈
3. 队列
4. 树
5. 排序

## python ##

### python的内存处理 ###

### python 垃圾回收###


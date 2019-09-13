---
title: "用 org mode 管理读过的书"
tags: ["emacs", "orgmode", "reading"]
date: 2019-09-13 16:00:56
layout: post
excerpt: 写了个小脚本把读过的书从豆瓣抓下来，然后整理成 org-mode 文件，用 org-mode 管理读过的书。
categories: programming
comments: true
---

## 缘起 ##

豆瓣会删除书的条目，八月的时候它们删除了[《香港简史》](https://www.amazon.com/%E9%A6%99%E6%B8%AF%E7%B0%A1%E5%8F%B2%EF%BC%88%E9%A6%99%E6%B8%AF%E5%8F%B2%E5%90%8D%E8%91%97%E8%AD%AF%E5%8F%A2%EF%BC%89-Chinese-%E9%AB%98%E9%A6%AC%E5%8F%AF-ebook/dp/B07F8Q2673)，我在上面的笔记基本付诸东流[^1]。有一段时间豆瓣经常500。这两件事让我觉得豆瓣并不是很可靠。所以我开始用 [org-mode](https://orgmode.org) 做笔记，用 github 同步备份。

后来我读到[卡尔维诺的《为什么读经典》](https://book.douban.com/subject/10555550/)，深感应该重新审视一下我读过的书，所以我写了个小脚本，把豆瓣读过列表的数据抓下来，整理成 org 文件。

## 代码工作 ##

代码工作分两部分：

1. 爬虫；
2. 组织成 org mode；

代码见：[yato](https://github.com/ZhangYet/vanguard/tree/master/yato)[^2]。

### 爬虫 ###

因为很简单，所以没有用 [scrapy](https://scrapy.org)，实际上就是循环调用豆瓣请求已读链接的，把 html 保存下来，然后用 [beautiful soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 解析一下（实际上就是搜 selector）[^3]。

### org 文件生成 ###

这次做的事情最大的坑是 [PyOrgMode](https://github.com/bjonnh/PyOrgMode)，首先它已经弃坑了，然后它输出 org 的 tag 时是有问题的。它会把多个 tag 输出成 `:tag0::tag1::tag2:` tag 之间多了一个冒号。我在本地把代码修了[^4]。

说一下之所以使用 org 有几个原因：

1. 我毕竟是 emacs 用户；
2. 我可以用 tag 方便地给书分类；
3. 那些永远不会在读的书[^5]可以轻松 archie；

简单来说就是 org mode 用起来就像一个简单易用的数据库一样。但其实有个缺点，就是这么多 org file，emacs 初始化的时候会要了老命。

## 成品 ##

最后把每一本书保存成一个单独的 org 文件，所有记录在我另一个 [repo](https://github.com/ZhangYet/vigna/tree/master/book_review/douban) 里面。

![效果](/images/manage_your_library_by_org.png)


## 脚注 ##

[^1]: 当然在微博上还能找到一些，不过有些大段的摘抄就真的没有了。

[^2]: 这个 repo 的名字意为「先锋」，这个子文件夹的名字意为「夜刀」，这些名字都来自《明日方舟》。

[^3]: 主要的原因是我不熟 scrapy，为了这么点事搞这么重的框架，不值当啊。

[^4]: 所以 python 和 go 这些语言有个好处就是方便找到源代码，如果我用的是 c/c++ 的静态库，源代码都看不见的话，那就抓黑了。

[^5]: 属于「年轻时犯的错」。

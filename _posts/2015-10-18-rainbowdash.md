---
layout: post
title:  "最近做的小系统：rainbowdash"
comments: true
---

# rainbowdash是啥？

其实，Rainbow Dash是My Little Pony里面最快的那匹天马。我用来命名这个小系统。主要希望它快——运行快、开发快。

我最初开发rainbowdash有两个目的：学习Flask框架；监控我负责的服务器上git repo的版本以及commit log等信息。

做了两周之后，我遇到一点小挫折，又发现了一些好玩的新东西。所以现在rainbowdash还要成为我的实验平台，我可以在上面快速实现我的想法，试用新的python module.

# 我都做了啥，或者说，遇到什么挫折。

刚开始的时候，我是希望通过pygit2等库读取非local的git repo的信息。但是后来我发现，这做不到Orz。

然后我希望通过ssh连接，发送命令到远程机上，之后我觉得这样的开销太大了（主要是维持着ssh连接，好像不太容易，也不太安全）。

最后我回到一个熟悉的思路上：定时运行ssh 命令，将git repo的信息写入到一张表里面。rainbowdash只需要将表里面的东西展示出来就行了。而定时更新表的部分就用celery和supervisor来做。不过现在还要从头建表，自己写类实现数据库的操作。真心麻烦。
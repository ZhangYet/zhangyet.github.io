---
title: "写一个 FIX 模拟器"
tags: ["programming", "go"]
date: 2019/04/25 12:31:10
layout: post
excerpt: 用 go 语言实现一个 FIX 协议模拟器
comments: true
categories: programming
---

来到新公司两周了，第一周时间是看代码，然后设计一个 FIX 模拟器。这周开始写。

写了5个工作日之后，项目初步成型。这个项目主要的难点有几个：

1. [FIX 协议](https://www.onixs.biz/fix-dictionary.html)比较操蛋，这个协议定义了证券买卖的标准通信协议，定义了各种操作的 message，每种 message 有自己的 field， 不同的 field 有不同的类型与合法值。
2. 整个调用流程比较复杂，看了两个项目的代码，其中有一个项目还是 C++ 的，订单状态比较复杂。
3. 要写的项目跟上游项目的对接还不熟悉。

主要的任务是扩展 FIX 协议的语法，扩展 FIX 协议的描述（因为我们用的协议在标准 FIX 协议基础上定义了更多了条件）。

最后确定基于 [quickfixgo](https://github.com/quickfixgo/quickfix) 实现，之所以选这个，是因为 quickfix 的 C++ 实现没办法在 Mac 上编译，另外我对 go 比较熟。主要的开发工作就两块：在 FIX 标准协议的基础上，扩展 conditional required 的语义（每个 field 在 message 里面，分 required/optional/conditional required， required 要求该 message 必须带某个 field， conditional required 要求如果 message 某些 field 设置了特殊的值，则该 field 是 required）；在扩展的基础上，增加校验 message 的逻辑。

`quickfixgo` 的代码里面，对 message 的校验由一个 interface `Validator` 完成。所以最初的时候，我以为只需要实现一个新的 interface 就行了。没想到大意了，这个 Validator 会在 `quickfixgo` 初始化 session （关于 session 我们以后可能还要再讨论一下）的时候，初始化赋值给 session，问题就是 session 是一个私有域。我没有什么办法替换它。我同事建议我直接在 vendor 里面改——当然其实我最初的想法是找类似 [monkey patching](https://en.wikipedia.org/wiki/Monkey_patch) 的办法，这个大概可以花两到三天吧。

第一天扩展了协议的 Spec。主要的开发在 `quickfixgo` datadictionary 的 Parse 方法上。我写了一个单元测试，但是有一个地方一直不能通过测试：field 的 required 字段一直读不对，打了断点之后发现，读到对应的 field 的时候，还是正确的，但读下一个 message 的时候，这个值莫名其妙地变了。我看这个字段其实是一个指针，所以怀疑它在别的地方别改写了。花了很久，才明白到我搞错了。

如前所述，FIX 协议在 `quickfixgo` 用一个 xml 文件描述，这个文件其实分了好几部分，其中一部分是 field 的定义，这部分定义了 field 的 name 和 tag ，还有对应的值的类型（字符串？还是数值？还是日期？），以及有效的取值范围，这部分对应 fieldType ，另一部分就是 message 的定义，每个 message 会有具体的 field 的定义，这部分对应 fieldDef。

我的问题就在于我把 required 字段定义在 fieldType 部分了，这部分其实是所有 message 都共用的指针，所以在我的测试里，后面的 message 里面的 fieldDef 会覆盖 required 字段。

第二天开始整合代码，然后把模拟器的实例跑起来（当然，能编译过我们就算是能跑起来了）。但是编译过程也有一点问题，首先第一个就是，我用了 `go mod`，而 `quickfixgo` 的代码，我是通过 `go mod vendor` 拉到本地的 vendor 文件夹，然后在文件夹里面修改的。这样当我要加一个新的依赖，并且更新 vendor 的时候， `quickfixgo` 的修改就白费了。所以最后决定把 `quickfixgo` fork 到我们的代码库里面，既然都魔改了，就魔改到底吧，豁出去了。周二周三就忙活干这个了，最后和上游系统打通了。

周四和周五都在想办法校验 message。这里的难点又回到第一点：这个协议实在太麻烦了。我首先在 `quickfixgo` 里面增加了 Validator 的单元测试，改了我增加的 Validator 的 bug （有个比较低级的错误我需要说明一下，`quickfixgo` 里面有两个 Validator 实现，一个叫 FixtValidator, 另一个叫 FixValidator，前者是为了兼容 1.1 版本的 Fix 协议实现的，而我偏偏在这个实现那里加了逻辑。幸好我开发的时候遵循了它的规范，改正这个错误没有太多问题）。然后我做了一个工具，希望可以通过日志里面的 message 解析出一个可读的版本。这个工具写起来当然不难，问题出在：message 是用 '\001' 分割不同的 field 的，如果我把日志里面的 message 粘贴到别的文件里面，这个分隔符就会丢掉，然后我就会获得一个 `out of index` panic。所以周五为这个 FIX 模拟器加一个 logger 将 message 输出到文件里面。目前想到，它还可以配合模板，输出一个完整的函数，从日志里面的 message 里面还原创建 message 的代码。

这就是我在新公司的第一个项目。总结如下：

1. 金融领域的协议好复杂。
2. `go mod` 很好用。
3. 项目小就是有好处，随时拆分，随时创建新的项目都方便。
4. 领域知识真是必不可少啊。

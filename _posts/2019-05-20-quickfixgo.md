---
title: "quickfixgo 源代码剖析"
tags: ["go", "programming", "fix"]
date: 2019-05-20 20:34:17
layout: post
excerpt: 解析 quickfixgo 源代码。
categories: programming
comments: true
---

[quickfixgo](https://github.com/quickfixgo/quickfix) 是 [FIX 协议](https://en.wikipedia.org/wiki/Financial_Information_eXchange)的 go 实现，本文是 v0.6.0 版本的源码阅读笔记。

## 基本架构 ##

quickfixgo 提供了 `Acceptor` 结构。启动 `Acceptor` 进程将会监听指定端口（由配置的 `SocketAcceptPort` 指定），在此之前，`Accecptor` 会创建 `Session` （数量由配置中的 `SESSION` 一节指定，每个 `SESSION` 由 `BeginString`, `SenderCompID` 和 `TargetCompID` 确定[^1]），`Session` 处理 FIX message 并返回处理结果（以 FIX message 的格式）。

![quickfixgo 基本架构](/images/quickfixgo.png "quickfixgo 基本架构")

如图所示，`Acceptor.Start()` 的时候，它会 fork 出 `goroutine`，在其中执行 `Session.Run()`。`Session` 会阻塞在一个 `select` 语句上，等待若干不同的 `chan` 输入数据。

初看 quickfixgo 源代码时，容易绕晕的是它的数据传递方式。在 `Session.Run` 之后，`Acceptor` 会监听 `netConn`，接收刀 FIX msg 时，创建两个 `chan` —— `msgIn` 与 `msgOut`。并将这两个 `chan` 发送到对应 `Session` 的 `chan admin`。`admin` 只会处理跟连接管理相关的信息，当它接收到 `connect` 类型的信息时，它会判断 `Session` 状态，然后将 `Session` 的 `chan MessageIn` 替换为传进来的 `chan MsgIn`[^2]。之后 `Acceptor` 会 fork 出 `goroutine`，让 `msgIn` 在一个 readLoop 中不断接收信息，`msgOut` 不断写信息（`Sesssion` 会把结果写到 `msgOut` 中）。

这就是 quickfixgo 的基本架构，重点在于数据通过不同的 `chan` 在应用内部流动，有点绕——你很难直接通过代码跳转看清楚。

## 信息处理 ##

`Session` 在 `chan messageIn` 取得 FIX message 之后，message 的处理工作由 `stateMachine` 接手，它首先检查时间（看是否在交易时间中），然后检查是已经连接。如果一切正常，它会从 `messagePool` 中取出一个 msg，然后用读取的 FIX message 数据填充这个 msg[^3]。

quickfixgo 的 `sessionState` 是 `interface{}` 。它定义了：

1. `inSession`;
2. `latentState`;
3. `logonState`;
4. `logoutState`;
5. `notSessionState`;
6. `resendState`;

<div class="mermaid">
graph TB
    latentState--connect-->logonState
    logonState--msgTypeError-->latentState
    logonState--RejectLogon-->latentState
    logonState--targetTooHigh-->resendState
    logonState--logonSuccessfully-->inSession
    inSession--logonMsg^handleLogonFailed-->logoutState
    inSession--handleLogout-->latentState
    inSession--handelResendRequest or handleSequenceReset or handle normally-->inSession
    inSession--processReject and targetTooHigh-->resendState
    inSession--targetTooLow or incorrectBeginString-->logoutState
</div>

这中间的状态转移还是蛮复杂的，上图并不能完整概括。概括来说就是初始状态是 `latentState` ，Connect 之后转入 `logonState`，在 `logonState` 中接收到 logon 信息，正常处理完（即完成登录流程）会进入 `inSession`。


## 脚注 ##

[^1]: 其实严格来说还有 SessionId 还有 targetSubID 等字段，但我懒得写下来了。

[^2]: 这是我觉得很 trick 的一个地方，但看明白了你会觉得很顺理成章。

[^3]: 传入来的 FIX message 其实是 bytes，取出来的 msg 会在 Parse 之前被清空，然后被 Parse 成对应的 msg。我不是很明白为什么这里不是直接 new 一个空的 msg。

---
title: "grpc 文档笔记"
tags: ["golang", "grpc", "http2"]
date: 2019-06-11 15:52:13
layout: post
excerpt: grpc 文档的笔记
categories: programming
comments: true
---

## gRPC over HTTP2 ##

该[文档](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md)描述了 [HTTP2](https://tools.ietf.org/html/rfc7540) 实现 gRPC。

HTTP2 会要求保留 header (以:开头的 header)在其他 header 之前，gRPC 实现应该在保留 header 后马上发 `Timeout` 头，并在发送 **Custom-Metadata** 之前发送 **Call-Definition**。

某些 gRPC 实现允许覆盖 **Path**，但是 gRPC 并不鼓励这种做法。`Content-Type` 需以 "application/grpc" 开头，否则会收到 [405](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/405)。**Custom-Metedata** 中有以 'grpc-' 开始的头，这是保留的，别乱用。**Custom-Metedata** 不保证顺序，除了用 ',' 分隔的重复的值。**ASCII-Value** 不允许有首尾的空格，这个文件还定义了如何计算 header size。如果 `Message-Encoding` 是空的，那么 `Compressed-Flag` 一定是 0。

该文档描述了 gRPC 使用 HTTP2 通信时一些用到的 header 和使用这些 header 的细节（好空泛的评论啊）。



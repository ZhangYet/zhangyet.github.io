---
layout: post
title: "go interface 补充说明"
comments: true
---

工作中遇到一个需求：对接口进行个性化配置。

现在的系统设计是这样的，大部分接口都需要返回一个 `arry`， 其中的元素是接口自定义的结构体 `XXXApiOutput`。最初的想法是做成像 `python` 的装饰器一样的实现，后来发现并不现实。退而求其次，希望有一个统一的处理方式，可以减少对接口的侵入式改造。

某个晚上想到的方法就是，针对某些个性化规则，定义 `interface{}`，然后处理这些 `interface{}`。 

举例来说，加入我的个性化规则需要根据 `Uuid` 处理。那么我可以定义如下的结构：

```
type UuidHandler interface{
	GetUuid() string
}

func Foo(input UuidHander) {
	// func body
}

func HandleUuidResource(input []UuidHandler) []UuidHandler {
	// func body
}
```

然后这个计划无情地失败了，这跟我的需求有关，最终发现这种实现方式对我的需求依然有一定距离。当然，最大的问题在于：**对于 `go` 函数来说，即使你为 `XXXApiOutput` 实现了 `GetUuid `方法，`[]XXXApiOutput` 和 `[]UuidHandler` 依然是不同的类型**。

详细的内容可以参考这篇博文: [Intro++ to Go Interfaces](https://npf.io/2014/05/intro-to-go-interfaces/)。

这篇博文为我们提供了一些较为深入的知识：

1. 假设我们的 `XXXApiOutput` 实现了 `GetUuid` 方法，而我们有一个函数 `Foo(input UuidHandler)`, 当我们传入类型为 `XXXApiOutput` 的参数的时候，我们传入了它的值，`go` 会根据入参的值，构造出一个 `UuidHandler` 的值。
2. 然而 `go` 并不会把 `[]XXXApiOutput` 转换为 `[]UuidHandler`（其实我觉得你要是把 arry 理解成元素的集合，你也可以设计成自动转换，但显然，`go` 的设计者并不是这样想的。
3. 看一个具体的例子，这个例子告诉我们，如果你的方法定义的时候，reciever 是指针，那么你在 `Foo` 中也应该传入一个地址。这是因为对于 `Api2Output`，你是把 `*Api2Output` 这个类型定义成了 `UudiHandler interface`。 【但是你可以直接调用 `api2Output.GetUuid()` 这是因为 `go` 帮你做了转换，这是何等操蛋。】

```
type Api1Output struct {}

type Api2Output struct {}

func (this Api1Output) GetUuid() string {
	// func body
}

func (this *Api2Output) GetUuid() string {
	// func body
}

var api1Output Api1Output
var api2Output Api2output

Foo(api1Output)
Foo(&api2Output) // 这里输入的是 &api2Output
```
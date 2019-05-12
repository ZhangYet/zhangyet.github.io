---
layout: post
title: "Spike 抓虫记"
comments: true
---

`spike` 是我实现的一个 `graphite_api` 的 proxy .

通过 `graphite_api` 查询 `whishper` 数据的时候，`query` 的形式可能会很复杂，涉及到函数嵌套等问题，所以 `spike` 其实只做了一个工作：从线上的 `graphite_api` 中读取数据，再调用 `graphite_api` 进行下一步的处理。实质上，从 `graphite_api` 读取的数据是时间戳－数据值点对，这些数据点对是 `graphite_api` 最终处理的结果，`spike` 读取这些数据之后，需要对它们作逆映射，转换成 `graphite_api` 定义的 `TimeSeries` 实例。 总的来说，可以把 `spike` 看作一个从其它线上 `whisper` 读取数据的 `graphite_api` （很惭愧，`spike` 只是做了一点微不足道的工作)。

昨天 @wzyboy 跟我提了一个 bug : 同样的查询语句直接查询 `graphite_api` 可以正常地获取数据，但是访问 `spike` 却会返回500错误。于是我开始排查原因。

这个问题有点诡异，因为我无法在本地复现。错误信息最终定位到 `graphite_api` 的 `doDrawImage` 函数，我最初想会不会是 `graphite_api` 的版本问题。我本地的 `graphite_api` 是1.1.2版本，而线上的是1.1.3。于是在依赖里面指定了版本，上了一个新的容器，发现问题并没有解决。

然后我又开了一个 `cell` 的测试环境，在上面启动 `spike`， 这个 `spike` 同样无法复现前面的错误。最后我只能钻进容器里面，找到容器中的 `graphite_api` 在可疑的地方插入 `print` 排查有错误的地方。

这个错误是发生在 `graphite_api` 的 `Graph` 类的 `drawText()` 函数里面，问题在于 `drawText()` 函数被很多别的函数调用了，我不知道它是在哪一个函数调用的时候出的错。最后我是用 `try catch` 强行让 `spike` "正常"地返回一个结果，再跟正确的结果（之所以可以这样做，是因为这个查询返回的是一个图，对比两个图其实并不难）对比，最后将错误定位到 `drawLabel()` 函数里面。这个函数的作用是画出图的横坐标。

定位到错误之后，我发现 `drawLabel()` 是用时间来做横坐标的，它会计算适当的时间点（一个 `datetime` 对象），然后调用 `strftime("%a %l%p")` 转换成字符串。但是线上容器进行到这一步的时候，传了一个空字符串到 `drawText()` 导致了函数出错。令我感到奇怪的是，为什么同样的函数调用，再线上会出错呢？我在容器里面开 `python` 解释器，发现线上的 `python` 就是将 `datetime` 对象转换成了空字符串了！

感谢 @wzyboy 他很快告诉我这是 `musl/libc` 的[问题](https://github.com/esmil/musl/blob/master/src/time/strptime.c)，总的来说，这个 bug 的问题是这样的：线上 `spike` 是用 `alpine` 作为容器镜像的，而 `alpine` 没有用 `glibc` 而是用了 `musl/libc` 。`musl/libc` 不支持 `%l` 这种日期格式（WHY！！！！）,最终导致了这个诡异的错误。

找到问题所在，修复就是小菜一碟了，将 `spike` 的镜像换回 `centos` 就解决了。

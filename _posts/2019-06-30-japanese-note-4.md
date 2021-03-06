---
title: "日语学习周记（至6月30日）"
tags: ["japanese"]
date: 2019-06-30 22:46:21
layout: post
excerpt: 这周跟着 B 站教程学完了《新编日语》的语音部分。后半周世间都在更新 gojuon。
categories: japanese
comments: true
---

## 语音 ##

终于把语音部分的课程听完了，但是很水。浊音未必能记住（主要是た行和さ行那几个相同读音的浊音老是记不住）。拗音也记得不熟。这些我决定先放着。先往下学吧。

## gojuon ##

然后就花了大部分时间将 anki 和 gojuon 整合在一起。为什么要这样做呢？因为输入一个 Anki 卡片太麻烦了。我上周做的卡片有三个 field: 汉字（假如有的话）、假名（一般是平假名，假如是外来词就是片假名）和汉语。每录入一张卡片，我至少要切换一次输入法。即使我先输入假名再统一输入汉语，还是很麻烦。还有一个问题就是如果我手抖输入错了，修正也很麻烦——这还得我知道我输入错。

所以我将 anki 和 gojuon 整合在一起了。我只需要在 gojuon 里面查一个词，gojuon 会自动往 anki 里面插入三张卡片（正面是日文、振假名和英文）。之所以可以开发这个功能，是因为 anki 有一个插件: [anki-connect](https://github.com/FooSoft/anki-connect)，它提供了 REST 接口，让外部程序直接跟 anki 交互（查询/增删 deck/model/note）。其实也考虑过直接写 akpg 文件，但是诡异的是 go 居然只有一个读 apkg 文件的库，没有写的。

晚上完成了 gojuon 0.3.0 的开发，可以顺利录入卡片了。这部分开发的难处主要是 go 没有一个像 python 的 requests 库那么方便的库，封装一个 REST api 非常难受，所以我拖拖拉拉，拖到今天才完成。

## 关于学习的一点想法 ##

我是一个容易放弃的人，热情来得快去得也快。虽然从我的日语学习周记来看，我花在写代码上时间比学日语的时间长，但是如果没有 gojuon 这个项目，我可能在第三周就放弃日语学习了——只要进度一不如意，我就容易打退堂鼓。也算是一种扬长避短的方案吧。

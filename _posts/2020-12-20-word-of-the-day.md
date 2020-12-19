---
title: "抓取 Merriam-Webster word of the day"
tags: ["english","programming"]
date: 2020-12-20 01:56:39
layout: post
excerpt: 「语言是我给学生们的第一件武器……——艾克撒罗斯」。
categories: programming
comments: true
---

我订阅了 [Merriam Webster 的 Word of the day](https://www.merriam-webster.com/word-of-the-day/calendar)。 

每天一个单词，给出单词的含义，简单介绍词源小知识，最重要的是有比较新比较正规的语料作为示例。我觉得这是学习英语的好材料。于是周末抽空写了点小脚本把所有的条目抓了下来（这个网站结构非常简单，用 [bs4](https://pypi.org/project/beautifulsoup4/) 随便解析下 html 即可），保存成 org-mode 文件。

有兴趣的朋友可以直接下载 [pdf](https://github.com/ZhangYet/vigna/releases/download/0.1.0/Merriam-Webster.Word.of.the.day.pdf)，也可以去看我抓好的 [org-mode 文件](https://github.com/ZhangYet/vigna/blob/master/books/word_of_the_day.org)。我使用 [latex.css](https://latex.now.sh/) 排版，不过也不一定排得很好看。

其实我是用不上这个 pdf 的，只是想到我家那边的中学，教学水平实在一般，所以我收集一下，准备发给以前的老师，希望对家乡的教育有所裨益。


---
title: "写 emacs lisp 应用"
tags: ["emacs", "elisp", "programming"]
date: 2019-05-13 16:23:55
layout: post
excerpt: 我又捡起 emacs 这门手艺，所以写一写相关心得（以及坑）。
comments: true
categories: programming
---

## 缘起 ##

重拾 `Emacs` 有几个个原因：

1. 换了公司，换了开发环境；
2. 休假的时候写了一个 `vscode` 插件：[kamui](https://github.com/ZhangYet/kamui);
3. EmacsTW 维护者说：[Jojo，我唔用 Emacs 啦！——当然我只是在玩梗](https://twitter.com/EmacsTW/status/1123085690160332800)，然后我去看了他写的[Emacs-101](https://github.com/emacs-tw/emacs-101-beginner-survival-guide)，我觉得他写得很好；

## 重新配置 Emacs ##

配置 `Emacs` 其实是一件很痛苦的事情，比如 Mac 下面的 `Meta` 键问题，这个问题本身不是问题，但是我真的想不到它在 Mac 自带的 terminal 和 Item2 下的解决方案是不同的，这中间还有一段[血肉史](https://twitter.com/dantezy2814/status/1122884259243421698)。

这次重新配置之前，我痛定思痛，决定先解决一个问题：拿 `Emacs` 来做什么。实际上，要把 `Emacs` 调教成一个及格的开发环境，是很困难的。在我还用 `Emacs` 做 python 开发的时候，我一直没有解决代码跳转的问题（是的，就是这么菜）。但是我也不打算再解决了，毕竟，我有 jetbrains 全家桶了。总而言之，如果只是把 `Emacs` 当作日常写作工具的话，我们可以少配置很多东西。

所以最后我模仿 kuanyui 的[配置](https://github.com/kuanyui/.emacs.d)，重新打造个性化配置。原则就是：只有必需的功能才加入配置当中。目前这个半成品在[这里](https://github.com/ZhangYet/sekiro)。

## 写 Elisp ##

不得不说，其实我还有一个野望，通过玩 `Emacs` 熟识 `elisp`，然后学 [Lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus)，当然这个心思绕得远了。

周末回家的时候，特意写了一个脚本去把之前写在 [gitpress](https://gitpress.io) 的博文的文件名改成符合 git-page 要求。不得不说，写得略痛苦：括号一多你就不知道该在哪里改了。

最后的成果长这个样子，懒得说了。

```elisp
(defun read-lines (filePath)
  "Return a list of lines of a file at filePath."
  (with-temp-buffer
    (insert-file-contents filePath)
    (split-string (buffer-string) "\n" t)))

(defun get-date-line (lineList)
  "Get the forth line of a file"
  (nth 1 (
	  split-string (nth 3 lineList) " ")))

(defun re-format-date (dateStr)
  "Re format the date string"
  (replace-regexp-in-string "\/" "-" dateStr))

(defun gen-new-name (fileName)
  "Generate new name for a file"
  (setq dateStr (get-date-line (read-lines fileName)))
  (setq prefix (re-format-date dateStr))
  (format "%s-%s" prefix fileName))

(defun rename (filePath)
  "Rename post file"
  (setq newFilePath (gen-new-name filePath))
  (rename-file filePath newFilePath))

(defun iter-dir-files (fileNames)
  (if fileNames
      (progn
	(setq current-file (car fileNames))
	(if (cl-search "md" current-file)
	    (rename current-file))
	(iter-dir-files (cdr fileNames)))))

(iter-dir-files (directory-files "."))

```


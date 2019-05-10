---
title: "go modules 简介"
tags: ["golang", "programming"]
date: 2019/04/08 20:02:49
excerpt: 介绍 go modules，本文是 go modules 官方文档的摘要笔记。
---

`go` 1.11 引入了 [modules](https://github.com/golang/go/wiki/Modules) 这个特性。本文是该文档的摘要笔记。

## Quickstart

官方的 [quickstart](https://github.com/golang/go/wiki/Modules#example) 给了一个例子，但是我在编译的时候遇到了错误：

```bash
go: golang.org/x/text@v0.0.0-20170915032832-14c0d48ead0c: unrecognized import path "golang.org/x/text" (https fetch: Get https://golang.org/x/text?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
go: error loading module requirements
```

云栖社区给了一个[解决方案](https://yq.aliyun.com/articles/663151)，但是我发现还是不行，最终的解决方案如下：

```bash
go mod edit -replace=golang.org/x/text@v0.0.0-20170915032832-14c0d48ead0c=github.com/golang/text@latest
```

这是因为 text 没有语义性版本，所以它的版本号是 [pseudo-versions](https://tip.golang.org/cmd/go/#hdr-Pseudo_versions)。

## 概念

1. `modules`: 不同的 package 组成的一个 versioned unit，简单来说就是一组 package， 但是作为整体带上了 version。 每个 module 都带有[语义性版本号](https://semver.org/)。
2. `go.mod`: 一个定义了整个 module 的文本文件，有四指令：`module`, `require`, `replace`, `exclude`。关于 `module`，文档说得很清楚，我不想概括复述了。

> A module declares its identity in its go.mod via the module directive, which provides the module path. The import paths for all packages in a module share the module path as a common prefix. The module path and the relative path from the go.mod to a package's directory together determine a package's import path.

3. `version selection`: 如果你通过 `go.mod` 里面的 `require` 引入了外部的 module，在 `go build` 的时候，`go.mod` 会自动选择最高的版本。假如你引入了某个 module A 要求它的版本是 v1.0.0。后来你再引入龄一个 module B，B 依赖于 v1.2.0 的 A，那么会根据最小版本原则，选择 v1.0.0。**这里我想吐槽一句，假如依赖的版本不兼容又不遵守 semver 的规范呢（比如上例中 A 的 v1.0.0 和 v1.2.0 有不兼容的版本，而我们又必须同时依赖 A 和 B 呢）？**
4. `semantic import versioning`: 就是说，module 必须遵循 [semver](https://semver.org/), v2 必须在 `go.mod` 的 `module` 中说明。

## 如何使用 module

### 定义 module

1. 如果项目目录在 `GOPATH` 之外，那么不需要特别操作，如果在 `GOPATH` 内，需要 `export GO111MODULE=on`。如果不加，会提示 `go: modules disabled inside GOPATH/src by GO111MODULE=auto; see 'go help modules'`。
2. 创建 module 定义，`go mod init`，这一步会把现有的依赖文件（[支持的格式](https://tip.golang.org/pkg/cmd/go/internal/modconv/?m=all#pkg-variables)）中的依赖项加到 `go.mod` 文件里面。`init` 命令可以根据 VCS 的信息，确定 module 路径，也可以自定义 module 路径，比如：`go mod init github.com/my/repo`。
3. `go build ./... && go test ./...`

### 依赖升级和降级

`go get` 会自动修改 `go.mod`。实际上，`go build`, `go test` 甚至 `go list` 都会自动增加新依赖。

`go get` 的几种情况（注意这些命令不但会更新 foo，还会更新 foo 的依赖）：

1. `go get -u foo` 使用最新的 minor 或 patch 版本；
2. `go get -u=patch foo` 使用最新的 patch 版本；
3. `go get foo` 使用最新版本；
4. `go get foo@e3702bed2` 和 `go get foo@'<v1.6.2'` 使用指定版本；

没有时候用语义版本的 module 会使用 [pseudo-versions](https://tip.golang.org/cmd/go/#hdr-Pseudo_versions)。

### release module

一般的发布就是

1. `go mod tidy`，因为其他会改动 `go.mod` 的命令都不会删除不用的依赖;
2. `go test all`，;
3. 将 `go.sum` 加到库里;

但是如果要发布的版本是 v2 以上，那就有点麻烦了。主要有一点：如果你的项目是从 v2.0.0 或以上的版本，开始用 module，那么最好把主版本号加1。原因是：

> Incrementing the major version in this case provides greater clarity to consumers of foo, allows for additional non-module patches or minor releases on the v2 series of foo if needed, and provides a strong signal for a module-based consumer of foo that different major versions result if you do import "foo" and a corresponding require foo v2.2.2+incompatible, vs. import "foo/v3" and a corresponding require foo/v3 v3.0.0.

有两种发布方式：主分支发布，或者增加一个版本子目录发布。module path 后面都要加上主版本号。

### publish a release

通过打 tag 来发布新版本，tag 应该是 prefix(module) + version。

## 迁移到 module

如果是 v2 或以后的版本，迁移会需要做更多的考量。

值得注意的是，如果以前你的文档要求的安装指令是：`go get -u`，那你应该去掉 `-u`。

此外还有几个小问题：

1. module 引用非 module 依赖：直接用就行，如果依赖是 v2 以上的版本，`require` 会在版本号后面加上 `+incompatible`。
2. 非 module 引用 module：v2 以下版本没有问题，1.9.7+, 1.10.3+ and 1.11 可以使用 v2 的 module， 之前的版本会要求 module 通过子文件夹的方式发布。


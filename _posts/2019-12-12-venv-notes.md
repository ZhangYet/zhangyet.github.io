---
title: "python 虚拟环境的简单说明"
tags: ["python", "venv"]
date: 2019-12-12 23:36:52
layout: post
excerpt: 理解 python virtual environment 是什么，如何起作用。
categories: programming
comments: true
---

## virtual environment 是什么 ##

从[官方文档](https://docs.python.org/3/library/venv.html#venv-def)摘录一个简单的定义：

> A virtual environment is a directory tree which contains Python executable files and other files which indicate that it is a virtual environment.

简单来说就是一个独立的环境（具体表现就是一个文件夹），里面包含了独立的 python 程序以及必要的 python 库，如果在这个环境下安装新的 python 库，新的库会被安装在这个文件夹下面。

所谓独立，是指在不同虚拟环境中，使用的 python 可执行文件是不同的（当然也有可能相同，因为虚拟环境下的 python 可能是 symbol link， 但是这对初学者来说不重要），使用的 python 库也是独立的。举例来说，你在虚拟环境 A 安装了库 a，当你退出虚拟环境或者切换到虚拟环境 B 的时候，再次调用库 a，那么你调用的就不是虚拟环境 A 中的库 a，可能是别的虚拟环境（假如你切换了），也可能是系统安装的库 a。

## venv ##

从 python 3.3 开始， [venv](https://docs.python.org/3/library/venv.html) 就是 python 的标准库了。关于这个库，我们可以看 [PEP-405](https://www.python.org/dev/peps/pep-0405/)。了解这个库的背景。

首先在 venv 之前，早就有第三方库（如 [virtualenv](http://www.virtualenv.org/)）干这事了。PEP-405 描述了为什么需要 venv。但是我没看出跟 virtualenv 的区别。

```bash
python3.7 -m venv venv-example # 用系统的 python 3.7 创建一个虚拟环境 venv-example

virtualenv -p python3.7 virtualenv-example # 用 virtualenv 创建一个虚拟环境，指定 python 版本为 3.7
```

查看两个虚拟环境的区别：

```bash
➜  temp cd venv-example && tree -L 3
.
├── bin
│   ├── activate
│   ├── activate.csh
│   ├── activate.fish
│   ├── easy_install
│   ├── easy_install-3.7
│   ├── pip
│   ├── pip3
│   ├── pip3.7
│   ├── python -> python3.7
│   ├── python3 -> python3.7
│   └── python3.7 -> /Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7
├── include
├── lib
│   └── python3.7
│       └── site-packages
└── pyvenv.cf
```

```bash
➜  temp cd virtualenv-example && tree -L 2 # 留意这里 tree 的参数是2，因为如果改成3，lib 下面会有很多 symbol link，为了节约篇幅，这里不列出来了。
.
├── bin
│   ├── activate
│   ├── activate.csh
│   ├── activate.fish
│   ├── activate_this.py
│   ├── easy_install
│   ├── easy_install-3.7
│   ├── pip
│   ├── pip3
│   ├── pip3.7
│   ├── python -> python3.7
│   ├── python-config
│   ├── python3 -> python3.7
│   ├── python3.7
│   └── wheel
├── include
│   └── python3.7m -> /Library/Frameworks/Python.framework/Versions/3.7/include/python3.7m
├── lib
│   └── python3.7
└── pip-selfcheck.json
```

两者创建的 virtual environment，大致相同，就是在 virtualenv 会在 include 和 lib/python3.7 目录下，有更多的 symbol link。稍微细节一点的话，两者中的 pip 版本是不一样的（virtualenv 会在创建的过程中重新安装 pip）。

即使阅读了 PEP-405，我还是不明白 venv 和 virtualenv 的区别，但我打算搁置这个议题。只有一个建议：

**如果使用3.3之后版本的 python，使用 venv。至少你可以省下安装包的工作。**

## venv 如何起作用 ##

首先我们需要知道 python 是如何寻找库的。python 需要首先确定 `sys.prefix` 的值。在 PEP-405 实现（即 python 3.3）之前，python 通常会在系统目录树中寻找 `os.py` (假设我们并没有设置 `PYTHONHOME` 变量的话) 。

在前文，我们从 venv 创建的文件夹中，我们可以看见 pyvenv.cfg 文件。PEP-405 告诉我们，在 python 3.3 之后，python 会在 python 程序「附近」（即 python 程序的同一目录或上一级目录）寻找 pyvenv.cfg 文件，寻找之后，就可以解析并读入该文件的内容，下面是前文生成的 pyvenv.cfg 文件。

```
home = /Library/Frameworks/Python.framework/Versions/3.7/bin
include-system-site-packages = false
version = 3.7.0
```

在找到 pyvenv.cfg 之后，[sys.prefix](https://docs.python.org/3/library/sys.html#sys.prefix) 的值会设定为 pyvenv.cfg 所在的文件夹，而 [sys.base_prefix](https://docs.python.org/3/library/sys.html#sys.base_prefix) 会被设置为 pyvenv.cfg 中的 home 的值。当然，如果找不到 pyvenv.cfg 那么 python 会按老方法找到 python 可执行文件的路径以及 site-packages 的位置。

从这个角度，我们可以给出 python virtual environment 的定义：

> Thus, a Python virtual environment in its simplest form would consist of nothing more than a copy or symlink of the Python binary accompanied by a pyvenv.cfg file and a site-packages directory.

 
所以，当我们进入（通过 `source bin/activate`，仔细看上面）**进入一个 venv 创建的虚拟环境之后，运行 python 时，pyvenv.cfg 使 python 改变了 `sys.prefix` 和 `sys.base_prefix`**。而 [site.py](https://github.com/python/cpython/blob/3.8/Lib/site.py)[^1] 会在载入的时候，将 `sys.prefix` 加入 `[sys.path](https://docs.python.org/3/library/sys.html#sys.path)`。python 在运行时，会根据 `sys.path` 搜索对应的 module，见 [the module search patch](https://docs.python.org/3/tutorial/modules.html#the-module-search-path)。

总结来说，venv 帮我们做的事情就是：

1. 将 python 解释器复制或者链接到一个新目录（即虚拟环境的 bin 目录）；
2. 在虚拟环境中生成 pyvenv.cfg 文件；
3. 生成 activate 脚本；

而其他工作则是 python 本身的机制完成的，在运行 activate 脚本之后，python 会运行虚拟环境中的 python 解释器，同时因为 pyvenv.cfg 的存在，python 会修改 `sys.prefix` 和 `sys.base_prefix`，后续所有 module search 工作都会优先在虚拟环境的目录下进行。

## activate ##

前面已经叙述了创建虚拟环境和进入虚拟环境之后的一些机制，中间缺失的一环是进入虚拟环境。

创建虚拟环境后，venv 会为我们生成 activate 脚本，这个脚本是一个 shell 脚本，非常难读[^2]

```bash
_OLD_VIRTUAL_PATH="$PATH"
PATH="$VIRTUAL_ENV/bin:$PATH"
export PATH
```

首先 activate 有一个 `deactivate()` 函数，这个函数会还原系统所有的设置，所以 activate 脚本主要的工作就是：记录旧的变量值，替换新的变量值，所以其实没啥好说的。只有一点比较 tricky: 将 `$VIRTURL_ENV/bin` 放在 `$PATH` 最前面，这样系统搜索可执行文件的时候，会先搜索虚拟环境下的 bin 目录。


## 脚注 ##

[^1]: 可以直接阅读代码，也可以阅读相关库的文档：<https://docs.python.org/3/library/site.html>。

[^2]: 所以我想鸽掉不谈。但是虚荣心和自尊心使我不能这样做。但我必须说，我讨厌 shell 脚本。

---
layout: post
title: "go interface type的一点人生经验"
comments: true
---

故事的起点是那本[The Go Programming Langueage](https://book.douban.com/subject/26337545/)中的一个例子:

```
func main() {

    var buf *bytes.Buffer
	if debug {
		buf = new(bytes.Buffer)
	}
	f(buf)
	if debug {
		fmt.Println("if debug!")
	}
}

func f(out io.Writer) {
	fmt.Println("enter f")
	if out == nil {
		fmt.Println("out is nl")
	} else {
		out.Write([]byte("done\n"))

	}
	fmt.Printf("type of out is %T\n", out)
}

```

这个例子原本用来说明Go里面，`interface type`可以包含一个`nil`指针，但是这个`interface type`本身并非`nil`。当然这个例子我改编了一点。但是最后输出的结果是这样的：

```
enter f
type of out is *bytes.Buffer
if debug
```

这就有点尴尬了。最后穿进去的out，既非`nil`亦非`not nil`。让我们加两行代码，看看我们到底存了什么东西进去.

```
fmt.Println("the type of out is: ", reflect.TypeOf(out))
fmt.Println("the value of out is: ", reflect.ValueOf(out))
```

这次的输出是:
```
the type of out is: *bytes.Buffer
the value of out is: &{[]}
the value of out is:  &{[100 111 110 101 10] 0 [0 0 0 0] [100 111 110 101 10 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0] 0}
```

因吹斯婷。

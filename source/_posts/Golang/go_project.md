---
title: Golang 项目工程结构
date: 2025-07-16 11:52:08
tags:
  - Go语言工程结构
categories:
  - Golang
---

# Golang 项目工程结构

配置好工作目录后，就可以编码开发了，在这之前，我们看下 go 的通用项目结构，这里的结构主要是源代码相应地资源文件存放目录结构。

### 1.1 gopath 目录

gopath 目录就是我们存储我们所编写源代码的目录。该目录下往往要有 3 个子目录：src，bin，pkg。

> src ---- 里面每一个子目录，就是一个包。包内是 Go 的源码文件
>
> pkg ---- 编译后生成的，包的目标文件
>
> bin ---- 生成的可执行文件。

### 1.2 第一个程序

.在 HOME/go 的目录下，(就是 GOPATH 目录里)，创建一个目录叫 src，然后再该目录下创建一个文件夹叫 hello，在该目录下创建一个文件叫 helloworld.go，并双击打开，输入以下内容：

```go
packge main

import "fmt"

func main() {
  fmt.Println("Hello World")
}
```

2 . 执行 go 程序
方式一：

```bash
go run helloworld.go
```

方式二:

```go
step1：打开终端：在任意文件路径下，运行:
​			go install hello

​也可以进入项目(应用包)的路径，然后运行：
​			go install
```

> 注意，在编译生成 go 程序的时，go 实际上会去两个地方找程序包：
> GOROOT 下的 src 文件夹下，以及 GOPATH 下的 src 文件夹下。

在程序包里，自动找 main 包的 main 函数作为程序入口，然后进行编译。

step2：运行 go 程序
​ 在/home/go/bin/下(如果之前没有 bin 目录则会自动创建)，会发现出现了一个 hello 的可执行文件，用如下命令运行:
​ ./hello

### 1.3 第一个程序的解释说明

#### 3.2.1 package

- 在同一个包下面的文件属于同一个工程文件，不用 import 包， 可以直接使用
- 在同一个包下面的所有文件的 package 名，都是一样的
- 在同一个包下面的文件 package 名都建议设为是该名目名，但也可以不是

#### 3.2.2 import

- `import "fmt"` 告诉 go 编译器这个程序需要使用`fmt`包的函数，fmt 包实现了格式化 IO(输入/输出)的函数
- 可以是相对路径也可以是绝对路径，推荐使用绝对路径(起始于工程根目录)

1. 点操作
   我们有时候会看到如下的方式导入包

```go
import (
  . "fmt"
)
```

点操作的含义就是这个包导入之后在你调用这个包的函数时，你可以省略前缀的包名，也就是前面你调用的 fmt.Println("hello world")可以省略的写成 Println("hello world")。

2. 别名操作
   别名操作顾名思义我们可以把包命名成另一个我们用起来容易记忆的名字

```go
import (
  f "fmt"
)
```

别名操作的话调用包函数时前缀变成了我们的前缀，即 f.Println("hello world") 3. \_操作
这个操作经常是让很多人费解的一个操作码，

```go
import (
  "database/sql"
  _ "github.com/ziutek/mymysql/godrv"
)
```

\_ 操作其实是引入该包，而不直接使用包里面的函数，而是调用了该包里面的 init 函数。

#### 3.2.3 main 包

main() , 是程勋运行的入口

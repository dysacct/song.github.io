---
title: Go 语言基础语法
date: 2025-07-17 15:30:54
tags:
  - Go 基础
categories:
  - Golang
---

# Go 语言基础语法

### 1.GO 语言注释

```go
// 单行注释
/*
多行
注释
*/
```

### 2.GO 语言变量

#### 2.1 变量就是变化的量

定义变量:

```go
package main
import "fmt"

func main() {

  var name string = "zhangsan"
  fmt.Println(name)
}
```

短变量声明: num := 100

```go
fun main() {
  num := 100
  name := "zhangsan"
  fmt.Println(num, name)
  fmt.Printf("%T,%T", num, name) // %T 打印变量类型
}
```

变量的内存地址：

```go
package main

import "fmt"

func main() {
    var num int
	num = 100
	fmt.Printf("num:%d,内存地址:%p", num, &num)  // &取地址符
  num = 200
  fmt.Printf("num:%d,内存地址:%p", num, &num)  // 地址会刷新，但会保持一致
}
```

变量交换

```go
package main

import "fmt"

func main() {
    var a,b int
	  a = 100
    b = 200
    a, b = b, a
	fmt.Println(b, a)

}
```

变量的作用域

```go
package main

import "fmt"

func test()(int, int){
    return 100 200  //  调用test 就返回值 100 200
}
func main() {
    // a, b = test()
    a, _ = test() // 赋给_的值 都将被抛弃
	fmt.Println(a)

}
```

#### 2.2 常量

常量中的数据类型只能是: 布尔型、数字型（整数型，浮点数型和复数）和字符串类型。

不可改变的量

```go
const identifier [type] = value

package main
func main(){
    const URL string = "www.baidu.com"  //  const是常量的标识符 ： 显示定义
    const URL = "WWW.taobao.com"  // 隐示定义、
    const a,b,c = 3.14, "song", false // 多个定义
    URL = "www.jd.com"  // URL是不会被改变的，
}

```

iota (常量计数器)

```go
package main

import (
	"fmt"
)

func main() {
	const (
		// 一组常量里面，如果没有默认初始值，默认和上一行一致
		a = iota // iota 默认等于0
		b = iota
		c = iota   // 就算不给b,c赋值也是默认的  自动加1
		d = "haha" // iota = 3
		e
		f = 100
		g
		h = iota
		i
		j
		k
	)
	fmt.Println(a, b, c, d, e, f, g, h, i, j, k)
}
//  0 1 2 haha haha 100 100 7 8 9 10 
```

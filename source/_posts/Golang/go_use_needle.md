---
title: Go 指针使用
date: 2025-07-23 16:23:37
tags:
  - 指针
categories:
  - Golang
---

## 指针的用法

```go
func main() {
	a := 10
	b := &a
	fmt.Println(*b)
	*b = 20
	fmt.Println(a)
	// 10 20
}
```

1. 在函数里修改传入的变量值

```go
func changeValue(x *int) {
  *x = 200
}

func main() {
  a := 100
  changeValue(&a)
  fmt.Println(a)
}
```

2. 提高性能（避免大对象复制）

```go
type User struct{
  Name string
  Age int
}
func PrintUser(u *User) {
  fmt.Println(u.Name, u.Age)
}

func main() {
  u := User{
    Name: "Tom",
    Age: 10,
  }
  PrintUser(&u)
  u.Name = "Jack"
  PrintUser(&u)
}
```

3. 和结构体方法有关
   如果需要修改结构体里的值，方法接收器要用指针

```go
type Counter struct {
  Count int
}

func (c *Counter) Add() {
  c.Count += 1
}

func main() {
  c := Counter{
    10,
  }
  c.Add()
  fmt.Println(c.Count)
  // 11
}
```

## 总结

| 用法         | 示例                  | 说明                     |
| ------------ | --------------------- | ------------------------ |
| 获取地址     | p := &a               | 获取变量 a 的地址        |
| 取地址的值   | \*p                   | 获取指针 p 指向的值      |
| 修改值       | \*p = 123             | 通过指针修改原变量       |
| 函数传指针   | func f(p \*int)       | 在函数中修改外部变量     |
| Gin 指针用法 | func(c \*gin.Context) | 操作请求上下文必须用指针 |

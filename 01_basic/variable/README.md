## 变量声明
多个变量定义建议用括号括起来
```go
var (
	v1 int
	v2 string
)
```

## 变量初始化
定义变量同时初始化
```go
var v3 string = "I am v3"
```

## 类型推导
自动根据字面量判断出左边变量的类型
```go
v4 := 1
v5 := "I am v5"
```
## 变量作用域
- 全局变量：定义在函数外部的变量，它在程序整个运行周期内都有效。 在函数中可以访问到全局变量。
- 函数局部变量：定义在函数内部，无法在该函数外使用
- 语句块局部变量: 定义在`if` `for`等内部，无法在语句块外使用

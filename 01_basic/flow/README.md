和`c/c++`语言相比，`golang`对流程控制语句进行了一些精简，比如省去了`c/c++`中的括号。对于`if/for`都是。


## `if/else`语句
基本格式如下:
```go
if condition {
    分支1
} else if condition {
    分支2
} else {
    分支3
}
```
使用`else if / else`时必须跟在`}`之后，否则将会报错

## `for`循环
基本格式如下：
```go
for 初始语句;条件表达式;结束语句 {
    循环体语句
}
```
示例：
```go
for i := 0; i < 10; i++ {
	fmt.Println(i)
}
// 无限循环
for {
    ...
}
```
## `for/range`语句
在`golang`中，对于数组、切片、`map`以及`channel`等数据结构，用`for/range`遍历起来会更方便

```go
	//遍历切片
	s := []int{1, 2, 3, 4, 5}
	for k, v := range s {
		fmt.Println(k, v)
	}

	//遍历map
	m := make(map[string]int)
	m["key1"] = 1
	m["key2"] = 2
	m["key3"] = 3
	for k, v := range m {
		fmt.Println(k, v)
	}
```
可忽略不想要的返回值，或 "`_`" 这个特殊变量。
```go
	s := "abc"
	// 忽略 2nd value，支持 string/array/slice/map
	for i := range s {
		println(s[i])
	}
	// 忽略 index
	for _, c := range s {
		println(c)
	}
	// 忽略全部返回值，仅迭代
	for range s {

	}
	
	m := map[string]int{"a": 1, "b": 2}
	// 返回 (key, value)
	for k, v := range m {
		println(k, v)
	}
```
## `switch`分支
需要特别说一下的是，和`c/c++`里的`switch`不同，`Go`语言可省略 `break`，默认自动终止。
```go
n := 2
switch n {
case 1:
    fmt.Println("case 1 hit!")
case 2:
    fmt.Println("case 2 hit!")
default:
    fmt.Println("no case hit!")
}
```

### `Type Switch`
`switch` 语句还可以被用于 `type-switch` 来判断某个 `interface` 变量中实际存储的变量类型。
```go
 switch x.(type){
   case type:
     statement(s)
   case type:
     statement(s)
   /* 你可以定义任意个数的case */
   default: /* 可选 */
     statement(s)
 }
```
## `select` 语句

`select` 语句类似于 `switch` 语句，但是`select`会随机执行一个可运行的`case`。如果没有`case`可运行，它将阻塞，直到有`case`可运行。

```go
select {
  case communication clause :
    statement(s);
  case communication clause :
    statement(s);
  /* 你可以定义任意数量的 case */
  default : /* 可选 */
    statement(s);
}
```

对于`select`有以下规则：
- 每个`case`都必须是一个通信
- 所有`channel`表达式都会被求值
- 所有被发送的表达式都会被求值
- 如果任意某个通信可以进行，它就执行；其他被忽略。
- 如果有多个`case`都可以运行，`select`会随机公平地选出一个执行。其他不会执行。否则：
  - 如果有`default`子句，则执行该语句。
  - 如果没有`default`字句，`select`将阻塞，直到某个通信可以运行；`Go`不会重新对`channel`或值进行求值。
```go
	var c1, c2, c3 chan int
	var i1, i2 int
	select {
	case i1 = <-c1:
		fmt.Printf("received ", i1, " from c1\n")
	case c2 <- i2:
		fmt.Printf("sent ", i2, " to c2\n")
	case i3, ok := (<-c3): // same as: i3, ok := <-c3
		if ok {
			fmt.Printf("received ", i3, " from c3\n")
		} else {
			fmt.Printf("c3 is closed\n")
		}
	default:
		fmt.Printf("no communication\n")
	}
```
`select`可以监听`channel`的数据流动
```go
select { // 不停的在这里检测
	case <-chanl:         // 检测有没有数据可以读
	// 如果chanl成功读取到数据，则进行该case处理语句
	case chan2 <- 1:      // 检测有没有可以写
	// 如果成功向chan2写入数据，则进行该case处理语句

	}
```

## 循环控制`Goto`、`Break`、`Continue`
- 三个语句都可以配合标签(`label`)使用
- 标签名区分大小写，定义以后若不使用会造成编译错误
- `continue`、`break`配合标签(`label`)可用于多层循环跳出
- `goto`是调整执行位置，与`continue`、`break`配合标签(`label`)的结果并不相同


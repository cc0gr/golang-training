## 指针

区别于`C/C++`中的指针，`Go`语言中的指针不能进行偏移和运算，是安全指针。

### `Go`语言中的指针

- `Go`语言中的函数传参都是值拷贝，当想要修改某个变量的时候，可以创建一个指向该变量地址的指针变量。

- 传递数据使用指针，而无须拷贝数据。
- 类型指针不能进行偏移和运算
-  `Go`语言中的指针操作只需要记住两个符号`&` （取地址）和 `*` （根据地址取值）。

#### 指针地址和指针类型

- 每个变量在运行时都拥有一个地址，这个地址代表变量在内存中的位置。`Go`语言中使用`&`字符放在变量前面对变量进行“取地址”操作

- `Go`语言中的值类型 （`int`、`float`、`bool`、`string`、`array`、 `struct`） 都有对应的指针类型，比如： `*int`、`*int64`、`*string `等

- 取变量指针的语法:

  ```go
  ptr := &v // v的类型为T
  ```
  
其中：

> `v` :    代表被取地址的变量，类型为`T`,`ptr`: 用于接收地址的变量，`ptr`的类型就为`*T`，称做`T`的指针类型。`*`代表指针。

```go
// 声明
var n int = 10
var ptr *int

// 空指针判断
if nil == ptr {
    fmt.Println("这是一个空指针哦！")
}

// 指针取值 
ptr = &n  // 取变量n的地址，将指针保存到ptr中
fmt.Println("变量n的地址是: ", ptr)

// 使用指针访问值
fmt.Printf("%d\n", *ptr)

// 打印ptr的类型
fmt.Printf("type:%T\n", ptr)
```

总结： 取地址操作符`&`和取值操作符 `*` 是一对互补操作符， `&` 取出地址， `*` 根据地址取出地址指向的值。

#### **指针作为函数的参数**
如果在函数中需要修改参数的值的话，可能就需要通过指针来进行传递了

```go
package main

import "fmt"

func swap1(n1 int, n2 int) {
	nTemp := n2
	n2 = n1
	n1 = nTemp
}

func swap2(n1 *int, n2 *int) {
	var nTemp int
	nTemp = *n2
	*n2 = *n1
	*n1 = nTemp
}

func main() {
	var n1 int = 100
	var n2 int = 200
    
    // 不使用指针无法在函数中修改n1,n2的值
	swap1(n1, n2)
	fmt.Println("n1:", n1, "\tn2:", n2)

    // 使用指针作为参数，才能成功交换n1,n2的值
	swap2(&n1, &n2)
	fmt.Println("n1:", n1, "\tn2:", n2)
}
```

#### 空指针

- 当一个指针被定义后没有分配到任何变量时，它的值为 `nil`
- 空指针的判断

```go
package main

import (
	"fmt"
	"testing"
)

func main() {
	var ptr *string

	fmt.Println(ptr)			// output : nil
	if ptr != nil {
		fmt.Println("非空")
	} else {
		fmt.Println("空值")
	}
}
```


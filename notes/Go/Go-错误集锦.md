# Go-错误集锦

新手初级错误[参考链接](https://blog.csdn.net/keets1992/article/details/92816775)

1、左大括号 { 一般不能单独放一行
在其他大多数语言中，{  的位置你自行决定。Go 比较特别，遵守分号注入规则（automatic semicolon injection）：编译器会在每行代码尾部特定分隔符后加 ; 来分隔多条语句，比如会在 ) 后加分号： 错误提示如下：
```go
./main.go: missing function body
./main.go: syntax error: unexpected semicolon or newline before {
```

2、未使用的变量和包
如果在函数体代码中有未使用的变量，则无法通过编译，不过全局变量声明但不使用是可以的。即使变量声明后为变量赋值，依旧无法通过编译，需在某处使用它。import 导入的包也是，未使用也会报错。错误提示如下：
```go
declared and not used；imported and not used
```

3、简短声明的变量只能在函数内部使用
[ myvar := 1 ] 这种形式只能在函数体内部使用，即只能是局部变量，全局变量不可以。且此变量之前未被申明。struct 的变量字段不能使用 := 来赋值。

4、显式类型的变量无法使用 nil 来初始化
nil 是 interface、function、pointer、map、slice 和 channel 类型变量的默认初始值。但声明时不指定类型，编译器也无法推断出变量的具体类型。

```Go
// 错误示例 
func main() { 
    var x = nil // error: use of untyped nil 
    _ = x
}
// 正确示例 
func main() { 
    var x interface{} = nil
    _ = x
}
```

5、允许对值为 nil 的 slice 添加元素，但对值为 nil 的 map 添加元素则会造成运行时 panic
```Go
// map 错误示例
func main() {
    var m map[string]int //如果是全局变量，那么下面正确申明就不要加冒号了。
    m["one"] = 1		// error: panic: assignment to entry in nil map
    // m := make(map[string]int)// map 的正确申明，分配了实际的内存
}

// slice 正确示例
func main() {
	var s []int
	s = append(s, 1)
}
```
6、string 类型的变量值默认为 "" 不是 nil。0 和 1 也不能表示 True 和 False。

7、在 C/C++ 中，数组（名）是指针。将数组作为参数传进函数时，相当于传递了数组内存地址的引用，在函数内部会改变该数组的值。在 Go 中，数组，字符串是值。作为参数传进函数时，传递的是字符串，数组的原始值拷贝，此时在函数内部是无法更新该数组和字符串的，如果要修改数组，应该传递指针。直接使用 slice：即使函数内部得到的是 slice 的值拷贝，但依旧会更新 slice 的原始数据（底层 array）
```Go
// 传址会修改原数据
func main() {
	x := [3]int{1,2,3} //[数字] 是数字，[]没有是切片。

	func(arr *[3]int) {
		(*arr)[0] = 7
		fmt.Println(arr)	// &[7 2 3]
	}(&x)
	fmt.Println(x)	// [7 2 3]
}

// 会修改 slice 的底层 array，从而修改 slice
func main() {
	x := []int{1, 2, 3}
	func(arr []int) {
		arr[0] = 7
		fmt.Println(x)	// [7 2 3]
	}(x)
	fmt.Println(x)	// [7 2 3]
}

```

 **问：如何理解 错误是业务过程的一部分，而异常不是？**
**答**：错误指的是可能出现问题的地方出现了问题，比如打开一个文件时失败，这种情况在人们的意料之中 ；而异常指的是不应该出现问题的地方出现了问题，比如引用了空指针，这种情况在人们的意料之外。可见，错误是业务过程的一部分，而异常不是 。Golang 中引入 error 接口类型作为错误处理的标准模式，如果函数要返回错误，则返回值类型列表中肯定包含 error。error 处理过程类似于 C 语言中的错误码，可逐层返回，直到被处理。Golang 中引入两个内置函数 panic 和 recover 来触发和终止异常处理流程，同时引入关键字 defer 来延迟执行 defer 后面的函数。
一直等到包含 defer 语句的函数执行完毕时，延迟函数（defer 后的函数）才会被执行，而不管包含 defer 语句的函数是通过 return 的正常结束，还是由于 panic 导致的异常结束。你可以在一个函数中执行多条 defer 语句，它们的执行顺序与声明顺序相反。
当程序运行时，如果遇到引用空指针、下标越界或显式调用 panic 函数等情况，则先触发 panic 函数的执行，然后调用延迟函数。调用者继续传递 panic，因此该过程一直在调用栈中重复发生：函数停止执行，调用延迟执行函数等。如果一路在延迟函数中没有 recover 函数的调用，则会到达该携程的起点，该携程结束，然后终止其他所有携程，包括主携程（类似于 C 语言中的主线程，该携程 ID 为1）。错误和异常从 Golang 机制上讲，就是 error 和 panic 的区别。很多其他语言也一样，比如 C++/Java，没有 error 但有 error，没有 panic 但有 throw。Golang 错误和异常是可以互相转换的：错误转异常，比如程序逻辑上尝试请求某个 URL，最多尝试三次，尝试三次的过程中请求失败是错误，尝试完第三次还不成功的话，失败就被提升为异常了。异常转错误，比如 panic 触发的异常被 recover 恢复后，将返回值中 error 类型的变量进行赋值，以便上层函数继续走错误处理流程。

**问：对于常量定义zero(const zero = 0.0)，zero是浮点型常量，这一说法是否正确？**
**答**：错误，是 float64。Go 语言的常量有个不同寻常之处。虽然一个常量可以有任意有一个确定的基础类型，例如 int 或 float64，或者是类似 time.Duration 这样命名的基础类型，但是许多常量并没有一个明确的基础类型。编译器为这些没有明确的基础类型的数字常量提供比基础类型更高精度的算术运算；你可以认为至少有 256bit 的运算精度。这里有六种未明确类型的常量类型，分别是无类型的布尔型、无类型的整数、无类型的字符、无类型的浮点数、无类型的复数、无类型的字符串。`fmt.Printf("%T",zero)` 可以查看变量类型。

面：Go 语言和 python 的区别？


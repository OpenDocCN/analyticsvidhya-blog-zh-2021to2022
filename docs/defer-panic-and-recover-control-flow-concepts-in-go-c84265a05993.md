# Go 中的延迟、死机和恢复控制流概念

> 原文：<https://medium.com/analytics-vidhya/defer-panic-and-recover-control-flow-concepts-in-go-c84265a05993?source=collection_archive---------1----------------------->

# 介绍

开发 Go 应用程序时，延迟、死机和恢复是基本概念。使用`defer`函数，您可以调用一个函数，但要等到其他函数执行后才能执行。如果你熟悉 Python 语言，`defer` 函数类似于 Python 中的`finally`关键字。
对于`panic`的 Go 应用程序来说，它必须达到一个不能再运行的点，因为它不知道该做什么。这可以由编译器触发，也可以在我们的代码中手动触发。
至于`recover`关键词，与`panic`密切相关。当一个程序进入恐慌状态时，我们有一个选项来帮助它恢复，我们使用`recover`关键字。

在本文中，您将了解到`defer`、`panic`和`recover`函数是如何工作的，它的属性和用例。

# 先决条件

1.  Golang 基础知识

# 推迟

`defer`语句使您能够阻止一个函数的执行，直到稍后，这取决于您试图完成的任务。
用一个真实世界的例子来说，每个男人都有蓄胡子的能力，但这种能力会因为某种原因一直保持到你长大。基本上，这就是`defer`的工作方式。

## 性能

1.  Defer 语句不运行函数，它运行函数调用。
2.  它遵循后进先出的顺序，即最后调用的`defer`语句将首先执行。

## 如何使用延期

```
package main
import("fmt")func main() {
         fmt.Println("one")
         defer fmt.Println("three")
         fmt.Println("two")}
```

输出:

```
one
two
three
```

注意`three`是最后打印的。这是因为在使用了`defer`的函数结束之前，不会调用任何以`defer`关键字开头的语句。

```
package main
import("fmt")func main() {
         defer fmt.Println("one")
         defer fmt.Println("two")
         defer fmt.Println("three")}
```

输出:

```
three
two
one
```

如上面的属性所述，第一个被调用的 defer 语句是最后一个被执行的。

## 恐慌

`panic`是一个内置函数，用于停止程序的控制流。这可以由编译器完成，也可以手动添加到代码流中。当您的应用程序进入无法恢复的状态时，我们会使用紧急语句。例如:试图将一个数除以 0。
当程序根本无法继续时，它们就会发生。

## 性能

1.  在执行一条*延期* 语句后会发生死机。
2.  运行 panic 语句后，程序结束。

## 如何利用恐慌

```
package main
import "fmt"func main(){
        fmt.Println("one")
        defer fmt.Println("three")
        panic("a panic happened")
        fmt.Println("four")}
```

输出:

```
one
three
panic: a panic happened
```

正如你在上面看到的，由于代码编译器中调用的恐慌，我们没有执行程序的其余部分。

**再比如:**

```
package mainimport "fmt"func main() {
 x := 0
 y := 20
 fmt.Println(y/x)
}
```

输出:

```
panic: runtime error: integer divide by zero

goroutine 1 [running]:
main.main()
	/tmp/sandbox064477089/prog.go:8 +0x11
```

上面的输出显示，defer 语句不是最后运行，而是在死机发生之前首先执行。注意:Go 编译器也可能导致死机。

# 恢复

`recover`功能用于从`panic`恢复功能。**恢复**只在**延迟**函数内部有用。在正常执行中，对 **recover** 的调用将返回`nil`，并且没有其他影响。一个`recover`函数应该总是在一个`defer`函数内部被调用，因为如果程序死机，延迟函数不会停止执行，所以`recover`函数会停止死机。

## 性能

1.  *恢复* 报表是用来出**恐慌**的
2.  在延迟函数中很有用。这是因为当应用程序崩溃时，除了延迟的函数之外，它不再执行程序的其余部分。

## 如何使用 Recov `er`

```
package mainimport "fmt"func main() {
 x := 0
 y := 20
 printAllOperations(x, y)
}func printAllOperations(x int, y int) {defer func() {
 // defer function to escape the panic when y/x runs
  if r := recover(); r != nil {
   fmt.Printf("Recovering from panic in printAllOperations error is: %v \n", r)
   fmt.Println("Proceeding to alternative flow skipping division.")
   printOperationsSkipDivide(x, y)
  }
 }()sum, divide, multiply := x+y, y/x, x*y
 fmt.Printf("sum=%v, divide=%v, multiply=%v \n", sum, divide, multiply)
}func printOperationsSkipDivide(x int, y int) {
 sum, multiply := x+y, y*x
 fmt.Printf("sum=%v, multiply=%v \n", sum, multiply)
}
```

输出:

```
Recovering from panic in printAllOperations error is: runtime error: integer divide by zero 
Proceeding to alternative flow skipping division.
sum=20, multiply=0
```

`20/0`在 Go 编译器中引起了混乱，因此它调用了 defer 函数，该函数打印了其余结果的结果

# 结论

在本教程中，我们能够了解如何在 Golang 应用程序中使用 defer、recover 和 panic 函数。我希望这个指南能帮助你用那些函数写出更好的 Golang。
# GO

容易上手、编译速度快、原生支持并发、垃圾回收、代码风格清晰


# 宏定义  
GOROOT: Go程序安装路径  
GOPATH: Go工作区，存放第三方代码  
GOBIN: Go可执行程序，引用包路径  
GOPRIVATE: 第三方私有包下载路径  


# 语言 & 框架

[语言](https://www.runoob.com/go/go-tutorial.html)  
[go-kit框架](https://gokit.io/)  
[grpc](https://grpc.io/docs/languages/go/quickstart/)  



# 优雅代码

- 统计耗时 
```
func timeCost() func() {
	start := time.Now()
	return func() {
		tc := time.Since(start)    
		fmt.Println("time cost = %v", tc)
	}
}

使用示例
defer timeCost()()   // 注意，是对 timeCost() 返回的函数进行延迟调用，因此需要加两对小括号


```

# golang浮点数计算利器 decimal 

decimal是为了解决Golang中浮点数计算时精度丢失问题而生的一个库，使用decimal库我们可以避免在go中使用浮点数出现精度丢失的问题。

github地址：https://github.com/shopspring/decimal

使用decimal的时候，切记浮点数计算所有数据的初始化必须通过decimal进行，否则还是会导致精度的丢失  

- 为什么浮点数是不精确的？ 

计算机内部数字采用二进制存储， 所以对于小数而言， 计算机只能用这些个 1/(2^n） 之和来表达十进制的小数。 而计算机不可能提供无限的空间让程序去存储这些二进制小数， 比如golang的double就是64位。



# 基础知识

## 常用方法

- defer

	defer 可以保证一些代码在函数或者方法返回之前被调用，即使方法没有正常执行完，发生了 panic，defer 后面的代码也会执行。
	在定义 defer 的时候，引用的外部参数会立刻被拷贝。

- panic

	panic 是 Go 的内置函数，可以打断当前的代码的正常执行流程，如果一个函数中出现panic，该函数后续的代码都会停止执行。但是会执行 函数 中的 defer 代码。	  
	panic 只会保证当前 goroutine 中的 defer 代码一定会执行，其他 goroutine 中的 defer 代码不保证能执行。

- recover

	recover可以从 panic 中恢复程序的正常执行。recover 需要和 defer 定义在一起。  
	recover 和 panic 需要在同一个 goroutine 使用，跨 goroutine 无法恢复应用。


## 数据结构

- context
	Go 语言中的 context.Context 的主要作用还是在多个 Goroutine 组成的树中同步取消信号以减少对资源的消耗和占用，虽然它也有传值的功能，但是这个功能我们还是很少用到。



- channel
	
	channle 本质上是一个数据结构——（队列），数据是先进先出。具有线程安全机制，多个go程访问时，不需要加锁，也就是说channel本身是线程安全的。channel是有类型的，如一个string类型的channel只能存放string类型数据。

- slice

	slice不是线程安全的

- map 
	
	map不是线程安全的


- 可比较类型与不可比较类型
	
	1. 指针类型可比较，比较的是内存地址。  
    2. channel类型可比较，比较的是内存地址。  
    3. 数组类型可比较，比较的是长度、元素类型以及每个元素是否相同。  
    4. 结构体类型可比较的前提是结构体成员字段全部可以比较，并且结构体成员字段类型、个数、顺序也需要相同，当结构体成员全部相等时，两个结构体相等。  
    5. 接口类型的T和V全相等时则相等，接口类型比较时，如果底层类型不可比较，则会发生 panic。 
    6. 2 个 nil 类型可能不相等，两个nil 只有在类型相同时才相等。  
    7. slice, map, function不可比较，但可以和nil比较。至于这三种类型为什么不可比较，Golang 社区没有给出官方解释，经过分析，可能是因为 比较的维度不好衡量，难以定义一种没有争议的比较规则。   
    8. reflect.DeepEqual(value 1, value 2)可以用来比较元素是否相等。   



##  GMP

- G：gorotine（协程）

- M：machine（内核线程）

- P：processor(调度器)	


- M0: M0是启动程序后编号为0的主线程，当程序启动的时候会调用runtime包启动，用来执行初始化和启动第一个G。  

- G0: G0 是每次启动一个 M 都会第一个创建的 gourtine，G0 仅用于负责调度的 G，G0 不指向任何可执行的函数，每个 M 都会有一个自己的 G0（负责调度该M绑定的P的本地队列中的G）。在调度或系统调用时会使用 G0 的栈空间，全局变量的 G0 是 M0 的 G0  







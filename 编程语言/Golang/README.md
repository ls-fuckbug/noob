# GO

容易上手、编译速度快、原生支持并发、垃圾回收、代码风格清晰


# 宏定义  
GOROOT: Go程序安装路径  
GOPATH: Go工作区，存放第三方代码  
GOBIN: Go可执行程序，引用包路径  
GOPRIVATE: 第三方私有宝包下载路径  


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

- channel
	
	channle 本质上是一个数据结构——（队列），数据是先进先出。具有线程安全机制，多个go程访问时，不需要加锁，也就是说channel本身是线程安全的。channel是有类型的，如一个string类型的channel只能存放string类型数据。

- slice

	slice不是线程安全的

- map 
	
	map不是线程安全的


##  GMP

- G：gorotine（协程）

- M：machine（内核线程）

- P：processor(调度器)	


- M0: M0是启动程序后编号为0的主线程，当程序启动的时候会调用runtime包启动，用来执行初始化和启动第一个G。  

- G0: G0 是每次启动一个 M 都会第一个创建的 gourtine，G0 仅用于负责调度的 G，G0 不指向任何可执行的函数，每个 M 都会有一个自己的 G0（负责调度该M绑定的P的本地队列中的G）。在调度或系统调用时会使用 G0 的栈空间，全局变量的 G0 是 M0 的 G0  







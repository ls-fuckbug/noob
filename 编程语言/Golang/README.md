# GO

容易上手、编译速度快、原生支持并发、垃圾回收、代码风格清晰

Golang的相对路径是相对于执行命令时的目录


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
	延迟函数执行按照后进先出的顺序执行，即先出现的 defer 最后执行。


- panic

	panic 是 Go 的内置函数，可以打断当前的代码的正常执行流程，如果一个函数中出现panic，该函数后续的代码都会停止执行。但是会执行 函数 中的 defer 代码。	  
	panic 只会保证当前 goroutine 中的 defer 代码一定会执行，其他 goroutine 中的 defer 代码不保证能执行。

- recover

	recover可以从 panic 中恢复程序的正常执行。recover 需要和 defer 定义在一起。  
	recover 和 panic 需要在同一个 goroutine 使用，跨 goroutine 无法恢复应用。

- select

	go 的 select 为 golang 提供了多路 IO 复用机制，和其他 IO 复用一样，用于检测是否有读写事件是否 ready。linux 的系统 IO 模型有 select，poll，epoll，go 的 select 和 linux 系统 select 非常相似。	

- sync.WaitGroup

	Go语言中可以使用sync.WaitGroup来实现并发任务的同步。
    sync.WaitGroup内部维护着一个计数器，计数器的值可以增加和减少。例如当我们启动了N 个并发任务时，就将计数器值增加N。每个任务完成时通过调用Done()方法将计数器减1。通过调用Wait()来等待并发任务执行完，当计数器值为0时，表示所有并发任务已经完成。	


## 数据结构

### context
	
Go 语言中的 context.Context 的主要作用还是在多个 Go routine 组成的树中同步取消信号以减少对资源的消耗和占用，虽然它也有传值的功能，但是这个功能我们还是很少用到。


### channel
	
channel 是一个通道，用于端到端的数据传输，这有点像我们平常使用的消息队列，只不过 channel 的发送方和接受方是 goroutine 对象，属于内存级别的通信。

goroutine 是轻量级的协程，有属于自己的栈空间。 我们可以把它理解为线程，只不过 goroutine 的性能开销很小，并且在用户态上实现了属于自己的调度模型。

Go语言的并发模型是CSP（Communicating Sequential Processes），提倡通过通信共享内存而不是通过共享内存而实现通信。如果说goroutine是Go程序并发的执行体，channel就是它们之间的连接。channel是可以让一个goroutine发送特定值到另一个goroutine的通信机制。

Channel是Go中的一个核心类型，你可以把它看成一个管道，通过它并发核心单元就可以发送或者接收数据进行通讯(communication)。

Channel提供了一种同步的机制，确保在数据发送和接收之间的正确顺序和时机。通过使用channel，我们可以避免在多个goroutine之间共享数据时出现的竞争条件和其他并发问题。

Go 语言中的通道（channel）是一种特殊的类型。通道像一个传送带或者队列，总是遵循先入先出（First In First Out）的规则，保证收发数据的顺序。每一个通道都是一个具体类型的导管，也就是声明channel的时候需要为其指定元素类型。


#### 为什么channel是线程安全的？

channel内部维护了一个互斥锁，保证线程安全、先进先出。

#### 无缓冲的channel

一旦有 goroutine 往 channel 发送数据，那么当前的 goroutine 会被阻塞住，直到有其他的 goroutine 消费了 channel 里的数据，才能继续运行。

#### 有缓冲的channel

只要当前 channel 里的元素总数不大于这个可缓冲容量，则当前的 goroutine 就不会被阻塞住。

#### 已关闭的channel

当 channel 被关闭后，如果继续往里面写数据，则程序会直接 panic 退出。

不过读取关闭后的 channel，不会产生 pannic，还是可以读到数据。

如果关闭后的 channel 没有数据可读取时，将得到零值，即对应类型的默认值。

#### deadlock

往 channel 里读写数据时是有可能被阻塞住的，一旦被阻塞，则需要其他的 goroutine 执行对应的读写操作，才能解除阻塞状态。

然而，阻塞后一直没能发生调度行为，没有可用的 goroutine 可执行，则会一直卡在这个地方，程序就失去执行意义了。此时 Go 就会报 deadlock 错误


#### 从channel中取值

- for range

- select




### slice

slice不是线程安全的

nil切片和空切片不一样，nil切片指向的地址不存在，而所有空切片固定指向一个zero数组

append增加元素时，若容量不够，则会生成一个新切片，ptr指向一个新的切片

当传递切片给函数时，并且在函数中通过append方法向切片中增加值，当函数返回的时候，切片的值没有发生变化。
其实底层数组的值是已经改变了的（如果没有触发扩容的话），但是由于长度Len没有发生改变，所以显示的切片的值也没有发生改变


### map 
	
map不是线程安全的

 当往map中存储一个kv对时，通过k获取hash值，hash值的低八位和bucket数组长度取余，定位到在数组中的那个下标，hash值的高八位存储在bucket中的tophash中，用来快速判断key是否存在，key和value的具体值则通过指针运算存储，当一个bucket满时，通过overfolw指针链接到下一个bucket。



### rune

string底层是byte数组实现的，byte等同于int8, rune等同于int32

单引号，表示byte类型或rune类型，对应 uint8和int32类型，默认是 rune 类型。byte用来强调数据是raw data，而不是数字；而rune用来表示Unicode的code point。

双引号是字符串。


### 可比较类型与不可比较类型
	
1. 指针类型可比较，比较的是内存地址。  
2. channel类型可比较，比较的是内存地址。  
3. 数组类型可比较，比较的是长度、元素类型以及每个元素是否相同。  
4. 结构体类型可比较的前提是结构体成员字段全部可以比较，并且结构体成员字段类型、个数、顺序也需要相同，当结构体成员全部相等时，两个结构体相等。  
5. 接口类型的T和V全相等时则相等，接口类型比较时，如果底层类型不可比较，则会发生 panic。 
6. 2 个 nil 类型可能不相等，两个nil 只有在类型相同时才相等。  
7. slice, map, function不可比较，但可以和nil比较。至于这三种类型为什么不可比较，Golang 社区没有给出官方解释，经过分析，可能是因为 比较的维度不好衡量，难以定义一种没有争议的比较规则。   
8. reflect.DeepEqual(value 1, value 2)可以用来比较元素是否相等。   





#  GMP调度模型

## 为什么要设计GMP调度模型？

### 单进程时代

最初的单进程时代是不需要调度器的，因为一切的程序在操作系统上只能串行发生。

但是紧接着就带来两个问题：

- 单一的执行流程，计算机只能一个任务一个任务处理。  
- 进程阻塞所带来的CPU时间浪费。  

### 多进程并发

在多进程/多线程的操作系统中，就解决了阻塞的问题，因为一个进程阻塞cpu可以立刻切换到其他进程中去执行，而且调度cpu的算法可以保证在运行的进程都可以被分配到cpu的运行时间片。这样从宏观来看，似乎多个进程是在同时被运行。

进程拥有太多的资源，进程的创建、切换、销毁，都会占用很长的时间，CPU虽然利用起来了，但如果进程过多，CPU有很大的一部分都被用来进行进程调度了。

CPU调度切换的是进程和线程。尽管线程看起来很美好，但实际上多线程开发设计会变得更加复杂，要考虑很多同步竞争等问题，如锁、竞争冲突等。

### 协程的出现

一个线程分为“内核态“线程和”用户态“线程
内核线程依然叫“线程(thread)”，用户线程叫“协程(co-routine)".

1. N个协程绑定一个线程： 

优点：就是协程在用户态线程即完成切换，不会陷入到内核态，这种切换非常的轻量快速。  
缺点，1个进程的所有协程都绑定在1个线程上，一旦某协程阻塞，造成线程阻塞，本进程的其他协程都无法执行了，根本就没有并发的能力了

2. 1个协程绑定一个线程：

优点：这种最容易实现，协程的调度都由CPU完成了，不存在N:1缺点  
缺点：协程的创建、删除和切换的代价都由CPU完成，有点略显昂贵了  

3. M个协程绑定N个线程

是N:1和1:1类型的结合，克服了以上2种模型的缺点，但实现起来最为复杂
线程由CPU调度是抢占式的，协程由用户态调度是协作式的


### 为什么要有协程

协程（Coroutine）是一种轻量级的并发编程模型，它允许在单个线程内创建多个执行流程，可以在这些执行流程之间进行切换，从而实现并发处理。协程不同于传统的线程，它们更加轻量级，切换开销更低，可以更好地利用系统资源，以及更灵活地管理并发任务。

1. 轻量级： 协程是轻量级的执行单位，相比于操作系统线程更加节省内存和资源。  
2. 协作式调度： 协程的调度是协作式的，即协程在适当的时候自行挂起，并把控制权交给其他协程。这与操作系统线程的抢占式调度不同。  
3. 避免上下文切换： 协程之间的切换不需要像线程那样的昂贵上下文切换开销，因为切换是由协程自己管理的。   
4. 更高的并发性能： 协程的切换开销较小，使得在相同资源限制下可以创建更多的执行流程，从而提高并发性能。    
5. 简化并发编程： 协程模型可以将异步编程变得更加直观和易于理解，避免了传统回调式编程的复杂性。   



- G：gorotine（协程）

- M：machine（内核线程）

- P：processor(调度器)	


## 流程

1. 新建 G 时，新G会优先加入到 P 的本地队列；如果本地队列满了，则会把本地队列中一半的 G 移动到全局队列。

2. P 的本地队列为空时，就从全局队列里去取。

3. 如果全局队列为空时，M 会从其他 P 的本地队列偷（stealing）一半G放到自己 P 的本地队列。

4. M 运行 G，G 执行之后，M 会从 P 获取下一个 G，不断重复下去。



- M0: M0是启动程序后编号为0的主线程，当程序启动的时候会调用runtime包启动，用来执行初始化和启动第一个G。  

- G0: G0 是每次启动一个 M 都会第一个创建的 gourtine，G0 仅用于负责调度的 G，G0 不指向任何可执行的函数，每个 M 都会有一个自己的 G0（负责调度该M绑定的P的本地队列中的G）。在调度或系统调用时会使用 G0 的栈空间，全局变量的 G0 是 M0 的 G0  



# http/net

一次建立连接，就会启动一个读goroutine和写goroutine。

在获得resp.Body的内容后，不进行resp.Body.Close()，内存泄漏是一定的。


# 内存逃逸

golang程序变量会携带有一组校验数据，用来证明它的整个生命周期是否在运行时完全可知。如果变量通过了这些校验，它就可以在栈上分配。否则就说它 逃逸 了，必须在堆上分配。

能引起变量逃逸到堆上的典型情况：

- 在方法内把局部变量指针返回 

- 局部变量原本应该在栈中分配，在栈中回收。但是由于返回时被外部引用，因此其生命周期大于栈，则溢出。

- 发送指针或带有指针的值到 channel 中。 在编译时，是没有办法知道哪个 goroutine 会在 channel 上接收数据。所以编译器没法知道变量什么时候才会被释放。

- 在一个切片上存储指针或带指针的值。 尽管其后面的数组可能是在栈上分配的，但其引用的值一定是在堆上。  

- slice 的背后数组被重新分配了，因为 append 时可能会超出其容量( cap )。 slice 初始化的地方在编译时是可以知道的，它最开始会在栈上分配。如果切片背后的存储要基于运行时的数据进行扩充，就会在堆上分配。

- 在 interface 类型上调用方法。 在 interface 类型上调用方法都是动态调度的 —— 方法的真正实现只能在运行时知道。想像一个 io.Reader 类型的变量 r , 调用 r.Read(b) 会使得 r 的值和切片b 的背后存储都逃逸掉，所以会在堆上分配。


# 切片拷贝

拷贝大切片和拷贝小切片的消耗实际上一样，都是浅拷贝。（三个字段 Data Len Cap）

# 字符串转换为byte数组会发生内存拷贝吗？

字符串转成切片，会产生拷贝。严格来说，只要是发生类型强转都会发生内存拷贝。



# 垃圾回收GC

垃圾回收（Garbage Collection，简称 GC）是一种内存管理策略，由垃圾收集器以类似守护协程的方式在后台运作，按照既定的策略为用户回收那些不再被使用的对象，释放对应的内存空间.


## 几类经典的垃圾回收算法

### 标记清扫

标记清扫（Mark-Sweep）算法，分为两步走：

标记：标记出当前还存活的对象  

清扫：清扫掉未被标记到的垃圾对象  

这是一种类似于排除法的间接处理思路，不直接查找垃圾对象，而是标记存活对象，从而取补集推断出垃圾对象.  

至于标记清扫算法的不足之处，通过上图也得以窥见一二，那就是会产生内存碎片. 经过几轮标记清扫之后，空闲的内存块可能零星碎片化分布，此时倘若有大对象需要分配内存，可能会因为内存空间无法化零为整从而导致分配失败.

### 标记压缩

标记压缩（Mark-Compact）算法，是在标记清扫算法的基础上做了升级，在第二步”清扫“的同时还会对存活对象进行压缩整合，使得整体空间更为紧凑，从而解决内存碎片问题.

标记压缩算法在功能性上呈现得很出色，而其存在的缺陷也很简单，就是实现时会有很高的复杂度.


### 半空间复制

相信用过 Java 的同学对半空间复制（Semispace Copy）算法并不会感到陌生，它的核心点如下：

分配两片相等大小的空间，称为 fromspace 和 tospace

每轮只使用 fromspace 空间，以GC作为分水岭划分轮次

GC时，将fromspace存活对象转移到tospace中，并以此为契机对空间进行压缩整合

GC后，交换fromspace和tospace，开启新的轮次

显然，半空间复制算法应用了以空间换取时间的优化策略，解决了内存碎片的问题，也在一定程度上降低了压缩空间的复杂度. 但其缺点也同样很明显——比较浪费空间.


### 引用计数

引用计数（Reference Counting）算法是很简单高效的：

对象每被引用一次，计数器加1

对象每被删除引用一次，计数器减1

GC时，把计数器等于 0 的对象删除

然而，这个朴素的算法存在一个致命的缺陷：无法解决循环引用或者自引用问题.



## GOLANG的垃圾回收

### 三色标记法

Golang GC 中用到的三色标记法属于标记清扫-算法下的一种实现，由荷兰的计算机科学家 Dijkstra 提出，下面阐述要点：

- 对象分为三种颜色标记：黑、灰、白 

- 黑对象代表，对象自身存活，且其指向对象都已标记完成

- 灰对象代表，对象自身存活，但其指向对象还未标记完成

- 白对象代表，对象尙未被标记到，可能是垃圾对象

- 标记开始前，将根对象（全局对象、栈上局部变量等）置黑，将其所指向的对象置灰

- 标记规则是，从灰对象出发，将其所指向的对象都置灰. 所有指向对象都置灰后，当前灰对象置黑

- 标记结束后，白色对象就是不可达的垃圾对象，需要进行清扫.



### 并发垃圾回收

用户协程和GC协程并发操作的情况下可能会存在漏标和多标的情况


### 屏障机制

主要为了解决并发GC下的漏标问题

强三色不变式：白色对象不能被黑色对象直接引用

弱三色不变式：白色对象可以被黑色对象引用，但要从某个灰对象出发仍然可达该白对象


### 插入写屏障、删除写屏障、混合写屏障



# Golang的并发安全map实现

普通map没有内置的锁机制来保护多个 goroutine 同时对其进行读写操作。
当多个 goroutine 同时对同一个 map 进行读写操作时，就会出现数据竞争和不一致的结果。

## 读写锁

对整个map加读写锁，缺点是锁粒度太大


## 分片加锁

一个操作会导致整个map被锁住，导致性能降低。所以提出了分片思想，将一个map分成几个片，按片加锁。


## sync.Map

sync.Map底层使用了两个原生map，一个叫read，仅用于读；一个叫dirty，用于在特定情况下存储最新写入的key-value数据：


read map由于是原子包托管，主要负责高性能，但是无法保证拥有全量的key（因为对于新增key，会首先加到dirty中），所以read某种程度上，类似于一个key的快照。

dirty map拥有全量的key，当Store操作要新增一个之前不存在的key的时候，预先是增加自dirty中的。

在查找指定的key的时候，总会先去只读字典中寻找，并不需要锁定互斥锁。只有当read中没有，但dirty中可能会有这个key的时候，才会在锁的保护下去访问dirty。

在存储键值对的时候，只要read中已存有这个key，并且该键值对未被标记为“expunged”，就会把新值存到里面并直接返回，这种情况下也不需要用到锁。

更新，read和dirty因为value是指针，底层是一个value，这样都会被更新

对于新增的，会先加在dirty中，read中并不会新增

当删除的key在read中，可以通过软删除来标记，这样本身read对应的map不会因为频繁删除而触发等量扩容。

expunged和nil，都表示标记删除，但是它们是有区别的，简单说expunged是read独有的，而nil则是read和dirty共有的


read和dirty之间是会互相转换的，在dirty中查找key对次数足够多的时候，sync.Map会把dirty直接作为read，即触发dirty=>read升级。同时在某些情况，也会出现read=>dirty的重塑




sync.Map 的实现原理可概括为：
1. 通过 read 和 dirty 两个字段实现数据的读写分离，读的数据存在只读字段 read 上，将最新写入的数据则存在 dirty 字段上  
2. 读取时会先查询 read，不存在再查询 dirty，写入时则只写入 dirty  
3. 读取 和 修改 read 并不需要加锁，而读或写 dirty 则需要加锁  
4. 另外有 misses 字段来统计 read 被穿透的次数（被穿透指需要读 dirty 的情况），超过一定次数则将 dirty 数据更新到 read 中（触发条件：misses=len(dirty)）

### 优缺点

- 优点：Go官方所出；通过读写分离，降低锁时间来提高效率；  
- 缺点：不适用于大量写的场景，这样会导致 read map 读不到数据而进一步加锁读取，同时dirty map也会一直晋升为read map，整体性能较差，甚至没有单纯的 map+mutex 高。  
- 适用场景：读多写少的场景   



# CAS操作

CAS是Compare And Swap的缩写，直译就是比较并交换。CAS是现代CPU广泛支持的一种对内存中的共享数据进行操作的一种特殊指令，这个指令会对内存中的共享数据做原子的读写操作。其作用是让CPU比较内存中某个值是否和预期的值相同，如果相同则将这个值更新为新值，不相同则不做更新。

本质上来讲CAS是一种无锁的解决方案，也是一种基于乐观锁的操作，可以保证在多线程并发中保障共享资源的原子性操作，相对于synchronized或Lock来说，是一种轻量级的实现方案。



# map有哪些特点？

- Map 的键必须是可比较的类型，如整数、字符串和指针等，但是切片、函数和结构体等类型是不可比较的，因此不能用作键。  
- Map 中的元素是无序的，这意味着遍历 Map 时，元素的顺序可能会随机改变。  
- Map 的容量是动态变化的，它会自动调整容量以适应新的元素。  
- 如果使用未初始化的 Map，会导致运行时错误。需要使用 make() 函数来初始化 Map。  
- Map 在并发环境下不是安全的。如果需要在并发环境下使用 Map，需要使用 sync 包中提供的锁机制来保护 Map。  

# map扩容如何实现

- 当 Map 中的元素数量超过了负载因子（load factor）和哈希表容量的乘积时，map 就会触发扩容操作。在 Go 中，负载因子默认为 6.5。  
- Go Map 在扩容时会创建一个新的哈希表，并将原来的键值对重新散列到新的哈希表中。为了减少哈希冲突，新哈希表的容量是原来的两倍，并且容量一定是 2 的幂次方。  
- 在重新散列过程中，Go Map 会根据哈希值将原来的键值对分配到新哈希表中的对应位置上。如果两个键值对的哈希值相同，会使用链式哈希表（chained hash table）的方式进行处理，将它们放在同一个桶（bucket）中。  
- 一旦所有的键值对都已经重新散列到新的哈希表中，Go Map 就会将原来的哈希表释放掉，将新的哈希表作为 Map 的内部存储结构。  
 

# GO Profile

## 性能优化、调优原则

1. 要依靠数据而不是猜测  
2. 要定位最大瓶颈而不是细枝末叶  
3. 不要过早优化  
4. 不要过度优化  

Go 语言自带的 pprof 是一种性能分析工具，可以进行cpu、内存、死锁分析。 


profile: 画像，资料收集、剖析研究, 用于对程序指标或特征的分析

pprof : 是一种基于时间的采样方法，周期性地暂停程序并记录当前函数调用堆栈信息，以及每个函数调用的执行时间，这种采样方法可以提供关于函数调用频率和执行时间的统计信息，从而帮助分析程序的性能瓶颈



# 进程、线程、协程

- 进程：是应用程序的启动实例，进程拥有代码和打开的文件资源、数据资源、独立的内存空间，不同的进程通过进程间的通信方式来通信。

- 线程：从属于进程，每个进程至少包含一个线程，线程是 CPU 调度的基本单位，多个线程之间可以共享进程的资源并通过共享内存等线程间的通信方式来通信。
线程是最小的执行单元，进程是最小的资源管理单元。无论是进程还是线程，都是由操作系统所管理的。

- 协程：为轻量级线程，与线程相比，协程不受操作系统的调度，协程的调度器由用户应用程序(GMP)提供，协程调度器按照调度策略把协程调度到线程中运行。协程不是被操作系统内核所管理的，而是完全由程序所控制，也就是在用户态执行。这样带来的好处是性能大幅度的提升，因为不会像线程切换那样消耗资源。

## 对比

1. 协程既不是进程也不是线程，协程仅仅是一个特殊的函数，协程与进程和线程不是一个维度的。

2. 一个进程可以包含多个线程，一个线程可以包含多个协程。

3. 一个线程内的多个协程虽然可以切换，但是多个协程是串行执行的，只能在一个线程内运行，没法利用CPU多核能力。

4. 协程与进程、线程一样，切换是存在上下文切换问题的。

### 上下文切换的对比

1. 进程的切换者是操作系统，切换时机是根据操作系统自己的切换策略，用户是无感知的。进程的切换内容包括页全局目录、内核栈、硬件上下文，切换内容保存在内存中。进程切换过程是由“用户态到内核态到用户态”的方式，切换效率低。

2. 线程的切换者是操作系统，切换时机是根据操作系统自己的切换策略，用户是无感知的。线程的切换内容包括内核栈和硬件上下文,线程切换内容保存在内核栈中.线程切换过程是由“用户态到内核态到用户态”，切换效率中等。因为线程的调度是在内核态运行的，而线程中的代码是在用户态运行，因此线程切换会导致用户态与内核态的切换

每个进程都有属于自己的虚拟内存空间，进程中的所有线程共享进程的虚拟内存空间，所以进程之间互不影响，线程之间可能会相互影响。
线程切换比进程块的主要原因就是进程切换涉及虚拟内存地址空间的切换而线程不会。因为每个进程都有自己的虚拟内存地址空间，而线程之间的虚拟地址空间是共享的，因此同一个进程之中的线程切换不涉及虚拟地址空间的转换，然而将虚拟地址转化为物理地址需要查找页表，查找页表是一个很慢的过程，所以线程切换自然就比进程快了


3. 协程的切换者是用户（编程者或应用程序），切换时机是用户自己的程序所决定的。协程的切换内容是硬件上下文，切换内存保存在用户自己的变量（用户栈或堆）中。协程的切换过程只有用户态，即没有陷入内核态，因此切换效率高。


协程节约的时间的大小，取决于任务的IO耗时操作多不多，若IO操作多，协程越多，越省时。



# golang实现原子操作

原子操作就是不可中断的操作，外界是看不到原子操作的中间状态，要么看到原子操作已经完成，要么看到原子操作已经结束。在某个值的原子操作执行的过程中，CPU 绝对不会再去执行其他针对该值的操作，那么其他操作也是原子操作。

Go 语言的标准库代码包 sync/atomic 提供了原子的读取（Load 为前缀的函数）或写入（Store 为前缀的函数）某个值


# golang内存泄漏

go 中的内存泄漏一般都是 goroutine 泄漏，就是 goroutine 没有被关闭，或者没有添加超时控制，让 goroutine 一直处于阻塞状态，不能被 GC。


# golang哪些情况会导致panic？

1. 数组越界和字符串越界

2. 空指针引用

3. 断言失败

4. 并发访问map

5. 向已关闭的管道写数据会导致panic

6. stack overflow  递归死循环或超出栈空间








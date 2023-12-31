# Command

- who: 查看有没有其他用户在线


- 查找文件: find path -type d -name "xxx" 
	-type 指定类型    
	-name 文件名  

- which: 在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。  	
- 远程传输: scp (-r) src dst    

- 本地公钥复制至远程  ssh-copy-id dst 

- 建立符号链接: ln -s srcpath dstpath  


- 查看linux系统版本: uname -a  
- 查看计算机名: hostname
- 查看环境变量: env
- 查看内存使用量和交换区使用量: free -m
- 查看当前目录下的文件占用: du -h 
- 查看指定目录的大小: du -sh 目录名
- 查看系统运行时间、用户数、负载: uptime


- 查看端口号对应的进程: lsof -i : 50506   
- 查看网络接口属性: ifconfig
- 查看路由表: route -n
- netstat -lntp # 查看所有监听端口
- netstat -antp # 查看所有已经建立的连接
- netstat -s # 查看网络统计信息


- 实时显示进程状态: top



- 查看定时任务: crontab -l
- 程序后台运行： nohup   
- 关闭进程： ctrl + c  
- 暂停进程： ctrl + z  
- 将任务放入后台： bg %n   
- 设置任务忽略SIGHUB信号： disown -h %n   


# 基础知识

## 流文件描述符  
	
	标准输入(stdin) 文件描述符 0  
	标准输出(stdout) 文件描述符 1  
	标准错误输出(stderr) 文件描述符 2   



## 日志查询系列

cat/tac 适合查看行数比较少的文件  
	-n 列出行号  

more 翻页查看  
	空格 向下翻页  
	回车  向下翻行  
	/字符串  向下查找  
	:f  显示文件名及行号  
    b 往回翻页  

less 前后翻页  

	/字符串  向下查找  
    ?字符串  向上查找  
    n 重复前一个查找  
    g 到第一行  
    G 到最后一行  

head 查看前部
    -n 查看前n行  

tail 查看尾部  
   -n 查看后n行  


   
# 查看进程状态

ps aux 

```

USER：该 process 属于那个使用者账号的
PID ：该 process 的号码
%CPU：该 process 使用掉的 CPU 资源百分比
%MEM：该 process 所占用的物理内存百分比
VSZ ：该 process 使用掉的虚拟内存量 (Kbytes)
RSS ：该 process 占用的固定的内存量 (Kbytes)
TTY ：该 process 是在那个终端机上面运作，若与终端机无关，则显示 ?，另外， tty1-tty6 是本机上面的登入者程序，若为 pts/0 等等的，则表示为由网络连接进主机的程序。
STAT：该程序目前的状态，主要的状态有
R ：该程序目前正在运作，或者是可被运作
S ：该程序目前正在睡眠当中 (可说是 idle 状态)，但可被某些讯号 (signal) 唤醒。
T ：该程序目前正在侦测或者是停止了
Z ：该程序应该已经终止，但是其父程序却无法正常的终止他，造成 zombie (疆尸) 程序的状态

	 <    高优先级  
    N    低优先级  
    L    有些页被锁进内存  
    s    包含子进程  
    +    位于前台的进程组  
    l    多线程，克隆线程  multi-threaded (using CLONE_THREAD, like NPTL pthreads do)  

START：该 process 被触发启动的时间
TIME ：该 process 实际使用 CPU 运作的时间
COMMAND：该程序的实际指令

```   

孤儿进程： 一个父进程退出，而它的一个或多个子进程还在运行，那么这些子进程将成为孤儿进程。孤儿进程将被 init 进程(进程号为 1)所收养，并由 init 进程对它们完成状态收集工作。由于孤儿进程会被 init 进程收养，所以孤儿进程不会对系统造成危害。#


僵尸进程：  一个子进程的进程描述符在子进程退出时不会释放，只有当父进程通过 wait() 或 waitpid() 获取了子进程信息后才会释放。如果子进程退出，而父进程并没有调用 wait() 或 waitpid()，那么子进程的进程描述符仍然保存在系统中，这种进程称之为僵尸进程。  
僵尸进程通过 ps 命令显示出来的状态为 Z(zombie)。系统所能使用的进程号是有限的，如果产生大量僵尸进程，将因为没有可用的进程号而导致系统不能产生新的进程。要消灭系统中大量的僵尸进程，只需要将其父进程杀死，此时僵尸进程就会变成孤儿进程，从而被 init 所收养，这样 init 就会释放所有的僵尸进程所占有的资源，从而结束僵尸进程。#




# slurm

slurm是一个开源、容错和高度可扩展的集群管理和作业调度系统，适用于大型和小型 Linux 集群。Slurm 的运行不需要内核修改，并且相对独立。作为集群工作负载管理器，Slurm 具有三个关键功能。首先，它在一段时间内为用户分配对资源（计算节点）的独占和/或非独占访问权限，以便他们可以执行工作。其次，它为在分配的节点集上启动、执行和监控工作（通常是并行工作）提供了一个框架。最后，它通过管理待处理工作队列来仲裁资源的争用。   


- sbatch 向 SLURM 提交批处理脚本  
- squeue  列出当前正在运行或在队列中的所有作业  
- scancel  取消提交的工作  
- sinfo  检查所有分区中节点的可用性  
- scontrol  查看特定节点的配置或有关作业的信息  
- sacct  显示所有作业的数据  
- salloc	 预留交互节点  
  

  
# supervisor

supervisor主要的作用是管理进程，主要通过fork/exec程进对其启动（将其作为子进程），之后supervisor作为父进程对其启动，即使断电宕机也可将其重启，主要在配置文件中书写autostart=true即可。而传统方式的当即重启脚本需要写一个脚本来监控

supervisor启动的进程是由root创建，程序日志文件目录未指定会写在/下

使用 Supervisor 的最佳实践是为它将处理的每个程序编写一个配置文件。

在 Supervisor 下运行的所有程序都必须在非守护模式下运行（有时也称为“前台模式”）。如果默认情况下你的程序在运行后会自动返回到 shell，那么你可能需要查阅程序的手册来找到启用此模式的选项，否则 Supervisor 将无法正确确定程序的状态。

Supervisor程序的每个程序配置文件位于 /etc/supervisor/conf.d 目录中，通常每个文件运行一个程序，并以.conf 结尾。

管理程序： sudo supervisorctl


# 用户空间/内核空间

对32位操作系统而言，它的寻址空间（虚拟存储空间）为4G(2的32次方)  

 操作系统的核心是内核，独立于普通的应用程序，可以访问受保护的内存空间，也有访问底层硬件设备的所有权限。为了保证用户进程不能直接操作内核（kernel），保证内核的安全，操作系统将虚拟空间划分为两部分，一部分为内核空间，一部分为用户空间。

 针对linux操作系统而言，将最高的1G字节（从虚拟地址0xC0000000到0xFFFFFFFF），供内核使用，称为内核空间，而将较低的3G字节（从虚拟地址0x00000000到0xBFFFFFFF），供各个进程使用，称为用户空间。

 # 进程切换  

 为了控制进程的执行，内核必须有能力挂起正在CPU上运行的进程，并恢复以前挂起的某个进程的执行。这种行为被称为进程切换。因此可以说，任何进程都是在操作系统内核的支持下运行的，是与内核紧密相关的，并且进程切换是非常耗费资源的。

 从一个进程的运行转到另一个进程上运行，这个过程中经过下面这些变化：

1. 保存处理机上下文，包括程序计数器和其他寄存器。
2. 更新PCB（进程控制块）信息。
3. 把进程的PCB移入相应的队列，如就绪、在某事件阻塞等队列。
4. 选择另一个进程执行，并更新其PCB。
5. 更新内存管理的数据结构。
6. 恢复处理机上下文。

# 进程阻塞

正在执行的进程，由于期待的某些事件未发生，如请求系统资源失败、等待某种操作的完成、新数据尚未到达或无新工作做等，则由系统自动执行阻塞原语(Block)，使自己由运行状态变为阻塞状态。可见，进程的阻塞是进程自身的一种主动行为，也因此只有处于运行态的进程（获得了CPU资源），才可能将其转为阻塞状态。当进程进入阻塞状态，是不占用CPU资源的。

# 文件描述符

文件描述符（File descriptor）是计算机科学中的一个术语，是一个用于表述指向文件的引用的抽象化概念。
文件描述符在形式上是一个非负整数。实际上，它是一个索引值，指向内核为每一个进程所维护的该进程打开文件的记录表。当程序打开一个现有文件或者创建一个新文件时，内核向进程返回一个文件描述符。在程序设计中，一些涉及底层的程序编写往往会围绕着文件描述符展开。但是文件描述符这一概念往往只适用于UNIX、Linux这样的操作系统。

# 缓存IO

缓存I/O又称为标准I/O，大多数文件系统的默认I/O操作都是缓存I/O。在Linux的缓存I/O机制中，操作系统会将I/O的数据缓存在文件系统的页缓存中，即数据会先被拷贝到操作系统内核的缓冲区中，然后才会从操作系统内核的缓冲区拷贝到应用程序的地址空间。

数据在传输过程中需要在应用程序地址空间和内核进行多次数据拷贝操作，这些数据拷贝操作所带来的 CPU 以及内存开销是非常大的。


# 服务端处理网络请求的过程

## IO概念

I/O在计算机中指Input/Output， IOPS (Input/Output Per Second)即每秒的输入输出量(或读写次数)，是衡量磁盘性能的主要指标之一。IOPS是指单位时间内系统能处理的I/O请求数量，一般以每秒处理的I/O请求数量为单位，I/O请求通常为读或写数据操作请求。

一次完整的I/O是用户空间的进程数据与内核空间的内核数据的报文的完整交换，但是由于内核空间与用户空间是严格隔离的，所以其数据交换过程中不能由用户空间的进程直接调用内核空间的内存数据，而是需要经历一次从内核空间中的内存数据copy到用户空间的进程内存当中，所以简单说I/O就是把数据从内核空间中的内存数据复制到用户空间中进程的内存当中。

而网络I/O就是，从网络协议栈到用户空间进程内存的IO

磁盘I/O是进程向内核发起系统调用，请求磁盘上的某个资源比如是文件或者是图片，然后内核通过相应的驱动程序将目标图片加载到内核的内存空间，加载完成之后把数据从内核内存再复制给进程内存，如果是比较大的数据也需要等待时间。

## IO本质

网络I/O：本质是socket文件读取

磁盘I/O：本质是磁盘数据文件读取

每次I/O，都要经由两个阶段：

第一步：将数据从文件先加载至内核内存空间（缓冲区），等待数据准备完成，时间较长 

第二步：将数据从内核缓冲区复制到用户空间的进程的内存中，时间较短

## IO模型

同步/异步：关注的是消息通信机制

- 同步：synchronous，调用者等待被调用者返回消息，才能继续执行  
- 异步：asynchronous，被调用者通过状态、通知或回调机制主动通知调用者被调用者的运行状态

阻塞/非阻塞：关注调用者在等待结果返回之前所处的状态

- 阻塞：blocking，指IO操作需要彻底完成后才返回到用户空间，调用结果返回之前，调用者被挂起

- 非阻塞：nonblocking，指IO操作被调用后立即返回给用户一个状态值，无需等到IO操作彻底完成，最终的调用结果返回之前，调用者不会被挂起

Unix 有五种 I/O 模型：

1. 阻塞IO（bloking IO）  
2. 非阻塞IO（non-blocking IO）  
3. 多路复用IO（multiplexing IO）  
4. 信号驱动式IO（signal-driven IO）  
5. 异步IO（asynchronous IO）  

### 同步阻塞IO模型

#### 阻塞原理：

1、阻塞IO模型是最简单的IO模型，用户线程在内核进行IO操作时被阻塞。

2、用户线程通过系统调用read发起IO读操作，由用户空间转到内核空间。内核等到数据包到达后，然后将接收的数据拷贝到用户空间，完成read操作。

3、用户需要等待read将数据读取到buffer后，才继续处理接收的数据。整个IO请求的过程中，用户线程是被阻塞的，这导致用户在发起IO请求时，不能做任何事情，对CPU的资源利用率不够。

#### 优缺点：

优点：程序简单，在阻塞等待数据期间进程/线程挂起，基本不会占用 CPU 资源。
缺点：每个连接需要独立的进程/线程单独处理，当并发请求量大时为了维护程序，内存、线程切换开销较大，这种模型在实际生产中很少使用，apache的preforck使用的是这种模式。

#### 总结：

程序向内核发送IO请求后一直等待内核响应，如果内核处理请求的IO操作不能立即返回,则进程将一直等待并不再接受新的请求，并由进程轮训查看IO是否完成，完成后进程将IO结果返回给Client，在IO没有返回期间进程不能接受其他客户的请求，而且是有进程自己去查看IO是否完成，这种方式简单，但是比较慢，用的比较少。


### 同步非阻塞IO模型

#### 非阻塞原理：

用户线程发起IO请求时立即返回。但并未读取到任何数据，用户线程需要不断地发起IO请求，直到数据到达后，才真正读取到数据，继续执行。即 “轮询”机制存在两个问题：如果有大量文件描述符都要等，那么就得一个一个的read。这会带来大量的ContextSwitch（read是系统调用，每调用一次就得在用户态和核心态切换一次）。轮询的时间不好把握。这里是要猜多久之后数据才能到。等待时间设的太长，程序响应延迟就过大；设的太短，就会造成过于频繁的重试，干耗CPU而已，是比较浪费CPU的方式，一般很少直接使用这种模型，而是在其他IO模型中使用非阻塞IO这一特性。

#### 总结：

程序向内核发送请IO求后一直等待内核响应，如果内核处理请求的IO操作不能立即返回IO结果，进程将不再等待，而且继续处理其他请求，但是仍然需要进程隔一段时间就要查看内核IO是否完成。


### IO多路复用模型

#### IO多路复用原理：

1、IO多路复用（IO Multiplexing) ：是一种机制，程序注册一组socket文件描述符给操作系统，表示“我要监视这些fd是否有IO事件发生，有了就告诉程序处理”。对于监视的方式，又可以分为 select， poll， epoll三种方式。

2、IO多路复用是要和NIO一起使用的。NIO和IO多路复用是相对独立的。NIO仅仅是指IO API总是能立刻返回，不会被Blocking；而IO多路复用仅仅是操作系统提供的一种便利的通知机制。操作系统并不会强制这俩必须得一起用，可以只用IO多路复用 + BIO，这时还是当前线程被卡住。IO多路复用和NIO是要配合一起使用才有实际意义。

3、IO多路复用是指内核一旦发现进程指定的一个或者多个IO条件准备读取，就通知该进程。

4、多个连接共用一个等待机制，本模型会阻塞进程，但是进程是阻塞在select或者poll这样的系统调用上，而不是阻塞在真正的IO操作如recvfrom上。

5、用户首先将需要进行IO操作添加到select中，同时等待select系统调用返回。当数据到达时，IO被激活，select函数返回。用户线程正式发起read请求，读取数据并继续执行。

注意：Apache prefork是select模式，work是poll模式。

#### 优缺点

1、从流程上来看，使用select函数进行IO请求和同步阻塞模型没有太大的区别，甚至还多了添加监视IO，以及调用select函数的额外操作，效率更差。并且阻塞了两次，但是第一次阻塞在select上时，select可以监控多个IO上是否已有IO操作准备就绪，即可达到在同一个线程内同时处理多个IO请求的目的。而不像阻塞IO那种，一次只能监控一个IO。

2、虽然上述方式允许单线程内处理多个IO请求，但是每个IO请求的过程还是阻塞的（在select函数上阻塞），平均时间甚至比同步阻塞IO模型还要长。如果用户线程只是注册自己需要的IO请求，然后去做自己的事情，等到数据到来时再进行处理，则可以提高CPU的利用率。

3、IO多路复用是最常使用的IO模型，但是其异步程度还不够“彻底”，因它使用了会阻塞线程的select系统调用。因此IO多路复用只能称为异步阻塞IO模型，而非真正的异步IO。

### 信号驱动IO模型

#### 信号驱动IO模型原理：

1、用户进程可以通过sigaction系统调用注册一个信号处理程序，然后主程序可以继续向下执行，当有IO操作准备就绪时，由内核通知触发一个SIGIO信号处理程序执行，然后将用户进程所需要的数据从内核空间拷贝到用户空间。

2、此模型的优势在于等待数据报到达期间进程不被阻塞。用户主程序可以继续执行，只要等待来自信号处理函数的通知。

3、对于 TCP 而言，信号驱动的 I/O 方式近乎无用，因为导致这种通知的条件为数众多，每一个来进行判别会消耗很大资源，与前几种方式相比优势尽失。

#### 优缺点：

优点：线程并没有在等待数据时被阻塞，可以提高资源的利用率。
缺点：信号 I/O 在大量 IO 操作时可能会因为信号队列溢出导致没法通知。


### 异步IO模型

#### 异步IO原理：

1、异步IO与信号驱动IO最主要的区别是信号驱动IO是由内核通知应用程序何时可以进行IO操作，而异步IO则是由内核告诉用户线程IO操作何时完成。信号驱动IO当内核通知触发信号处理程序时，信号处理程序还需要阻塞在从内核空间缓冲区拷贝数据到用户空间缓冲区这个阶段，而异步IO直接是在第二个阶段完成后，内核直接通知用户线程可以进行后续操作了。

2、由 POSIX 规范定义，应用程序告知内核启动某个操作，并让内核在整个操作（包括将数据从内核拷贝到应用程序的缓冲区）完成后通知应用程序。

#### 优缺点：

优点：异步 I/O 能够充分利用 DMA 特性，让 I/O 操作与计算重叠。
缺点：要实现真正的异步 I/O，操作系统需要做大量的工作。目前 Windows 下通过 IOCP 实现了真正的异步 I/O，在 Linux 系统下，Linux 2.6才引入，目前 AIO 并不完善，因此在 Linux 下实现高并发网络编程时以 IO 复用模型模式+多线程任务的架构基本可以满足需求。

#### 总结

程序进程向内核发送IO调用后，不用等待内核响应，可以继续接受其他请求，内核调用的IO如果不能立即返回，内核会继续处理其他事物，直到IO完成后将结果通知给内核，内核在将IO完成的结果返回给进程，期间进程可以接受新的请求，内核也可以处理新的事物，因此相互不影响，可以实现较大的同时并实现较高的IO复用，因此异步非阻塞是使用最多的一种通信方式。



# IO多路复用 

- IO 多路复用是一种同步IO模型，实现一个线程可以监视多个文件句柄；
- 一旦某个文件句柄就绪，就能够通知应用程序进行相应的读写操作；
- 没有文件句柄就绪就会阻塞应用程序，交出CPU。

## 没有IO多路复用的情况

BIO同步阻塞 

服务端采用单线程，当 accept 一个请求后，在 recv 或 send 调用阻塞时，将无法 accept 其他请求（必须等上一个请求处理 recv 或 send 完 ）（无法处理并发）


NIO同步非阻塞  

服务器端当 accept 一个请求后，加入 fds 集合，每次轮询一遍 fds 集合 recv (非阻塞)数据，没有数据则立即返回错误，每次轮询所有 fd （包括没有发生读写实际的 fd）会很浪费 CPU。


## 多路复用三种实现

服务器端采用单线程通过 select/poll/epoll 等系统调用获取 fd 列表，遍历有事件的 fd 进行 accept/recv/send ，使其能支持更多的并发连接请求。


### select

它仅仅知道了，有I/O事件发生了，却并不知道是哪几个流（可能有一个，多个，甚至全部），我们只能无差别轮询所有流，找出能读出数据，或者写入数据的流，对他们进行操作。所以select具有O(n)的无差别轮询复杂度，同时处理的流越多，无差别轮询时间就越长。


缺点： 

select本质上是通过设置或者检查存放fd标志位的数据结构来进行下一步处理。这样所带来的缺点是：

单个进程所打开的FD是有限制的，通过 FD_SETSIZE 设置，默认1024 ;

每次调用 select，都需要把 fd 集合从用户态拷贝到内核态，这个开销在 fd 很多时会很大；

对 socket 扫描时是线性扫描，采用轮询的方法，效率较低（高并发）

select采取了内存拷贝方法来实现内核将 FD 消息通知给用户空间，这样一个用来存放大量fd的数据结构，这样会使得用户空间和内核空间在传递该结构时复制开销大

### poll

poll本质上和select没有区别，它将用户传入的数组拷贝到内核空间，然后查询每个fd对应的设备状态， 但是它没有最大连接数的限制，原因是它是基于链表来存储的.

poll是“水平触发”，如果报告了fd后，没有被处理，那么下次poll时会再次报告该fd

select是“边缘触发”，即只通知一次

缺点： 

每次调用 poll ，都需要把 fd 集合从用户态拷贝到内核态，这个开销在 fd 很多时会很大；

对 socket 扫描是线性扫描，采用轮询的方法，效率较低（高并发时）


### epoll 

epoll可以理解为event poll，不同于忙轮询和无差别轮询，epoll会把哪个流发生了怎样的I/O事件通知我们。所以我们说epoll实际上是**事件驱动（每个事件关联上fd）**的，此时我们对这些流的操作都是有意义的。（复杂度降低到了O(1)）

每一个epoll对象都有一个独立的eventpoll结构体，用于存放通过epoll_ctl方法向epoll对象中添加进来的事件。这些事件都会挂载在红黑树中，如此，重复添加的事件就可以通过红黑树而高效的识别出来(红黑树的插入时间效率是lgn，其中n为红黑树元素个数)。

而所有添加到epoll中的事件都会与设备(网卡)驱动程序建立回调关系，也就是说，当相应的事件发生时会调用这个回调方法。这个回调方法在内核中叫ep_poll_callback,它会将发生的事件添加到rdlist双链表中。

当调用epoll_wait检查是否有事件发生时，只需要检查eventpoll对象中的rdlist双链表中是否有epitem元素即可。如果rdlist不为空，则把发生的事件复制到用户态，同时将事件数量返回给用户。

树和双链表数据结构，并结合回调机制，造就了epoll的高效。

三步操作：

第一步：epoll_create()系统调用。此调用返回一个句柄，之后所有的使用都依靠这个句柄来标识。

第二步：epoll_ctl()系统调用。通过此调用向epoll对象中添加、删除、修改感兴趣的事件，返回0标识成功，返回-1表示失败。

第三部：epoll_wait()系统调用。通过此调用收集收集在epoll监控中已经发生的事件。


优点： 

没有最大并发连接的限制，能打开的FD的上限远大于1024（1G的内存上能监听约10万个端口）；

效率提升，不是轮询的方式，不会随着FD数目的增加效率下降。只有活跃可用的FD才会调用callback函数；即Epoll最大的优点就在于它只管你“活跃”的连接，而跟连接总数无关，因此在实际的网络环境中，Epoll的效率就会远远高于select和poll；

内存拷贝，利用mmap(Memory Mapping)加速与内核空间的消息传递；即epoll使用mmap减少复制开销。


两种触发方式：

LT（水平触发）模式下，只要这个 fd 还有数据可读，每次 epoll_wait 都会返回它的事件，提醒用户程序去操作；

ET（边缘触发） 模式下，它只会提示一次，直到下次再有数据流入之前都不会再提示了，无论 fd 中是否还有数据可读。所以在 ET 模式下，read 一个 fd 的时候一定要把它的 buffer 读完，或者遇到 AGAIN 错误。


epoll使用“事件”的就绪通知方式，通过epoll_ctl注册fd，一旦该fd就绪，内核就会采用类似callback的回调机制来激活该fd，epoll_wait便可以收到通知。


### select/poll/epoll的区别 

select，poll，epoll都是IO多路复用的机制。I/O多路复用就通过一种机制，可以监视多个描述符，一旦某个描述符就绪（一般是读就绪或者写就绪），能够通知程序进行相应的读写操作。但select，poll，epoll本质上都是同步I/O，因为他们都需要在读写事件就绪后自己负责进行读写，也就是说这个读写过程是阻塞的，而异步I/O则无需自己负责进行读写，异步I/O的实现会负责把数据从内核拷贝到用户空间。

epoll是Linux目前大规模网络并发程序开发的首选模型。在绝大多数情况下性能远超select和poll。目前流行的高性能web服务器Nginx正式依赖于epoll提供的高效网络套接字轮询服务。但是，在并发连接不高的情况下，多线程+阻塞I/O方式可能性能更好。

redis是单线程的，所以也使用了多路复用，主要基于epoll。


# MMAP

mmap()系统调用使得进程之间通过映射同一个普通文件实现共享内存。普通文件被映射到进程地址空间后，进程可以向访问普通内存一样对文件进行访问。

传统的 Linux 系统的标准 I/O 接口（read、write）是基于数据拷贝的，也就是数据都是 copy_to_user 或者 copy_from_user，这样做的好处是，通过中间缓存的机制，减少磁盘 I/O 的操作，但是坏处也很明显，大量数据的拷贝，用户态和内核态的频繁切换，会消耗大量的 CPU 资源，严重影响数据传输的性能，统计表明，在Linux协议栈中，数据包在内核态和用户态之间的拷贝所用的时间甚至占到了数据包整个处理流程时间的57.1%。

mmap() 系统调用函数会直接把内核缓冲区里的数据「映射」到用户空间，这样，操作系统内核与用户空间就不需要再进行任何的数据拷贝操作。



# DMA 数据直接访问 （Direct Memory Access） 

在进行 I/O 设备（网卡、磁盘等）和内核的数据传输的时候，数据搬运的工作全部交给 DMA 控制器，而 CPU 不再参与任何与数据搬运相关的事情，这样 CPU 就可以去处理别的事务。  


# 零拷贝

读取磁盘数据的时候，之所以要发生上下文切换，这是因为用户空间没有权限操作磁盘或网卡，内核的权限最高，这些操作设备的过程都需要交由操作系统内核来完成，所以一般要通过内核去完成某些任务的时候，就需要使用操作系统提供的系统调用函数。而一次系统调用必然会发生 2 次上下文切换：首先从用户态切换到内核态，当内核执行完任务后，再切换回用户态交由进程代码执行。所以，要想减少上下文切换到次数，就要减少系统调用的次数。

## 传统的文件传输4次拷贝

1. 第一次拷贝，把磁盘上的数据拷贝到操作系统内核的缓冲区里，这个拷贝的过程是通过 DMA 搬运的。  
2. 第二次拷贝，把内核缓冲区的数据拷贝到用户的缓冲区里，于是我们应用程序就可以使用这部分数据了，这个拷贝到过程是由 CPU 完成的。  
3. 第三次拷贝，把刚才拷贝到用户的缓冲区里的数据，再拷贝到内核的 socket 的缓冲区里，这个过程依然还是由 CPU 搬运的。  
4. 第四次拷贝，把内核的 socket 缓冲区里的数据，拷贝到网卡的缓冲区里，这个过程又是由 DMA 搬运的。   


传统的文件传输方式会历经 4 次数据拷贝，而且这里面，「从内核的读缓冲区拷贝到用户的缓冲区里，再从用户的缓冲区里拷贝到 socket 的缓冲区里」，这个过程是没有必要的。因为文件传输的应用场景中，在用户空间我们并不会对数据「再加工」，所以数据实际上可以不用搬运到用户空间，因此用户的缓冲区是没有必要存在的。

## 如何优化上下文切换

要想提高文件传输的性能，就需要减少「用户态与内核态的上下文切换」和「内存拷贝」的次数。

读取磁盘数据的时候，之所以要发生上下文切换，这是因为用户空间没有权限操作磁盘或网卡，内核的权限最高，这些操作设备的过程都需要交由操作系统内核来完成，所以一般要通过内核去完成某些任务的时候，就需要使用操作系统提供的系统调用函数。

而一次系统调用必然会发生 2 次上下文切换：首先从用户态切换到内核态，当内核执行完任务后，再切换回用户态交由进程代码执行。

所以，要想减少上下文切换的次数，就要减少系统调用的次数。


## 零拷贝概念

所谓的零拷贝（Zero-copy）技术，因为我们没有在内存层面去拷贝数据，也就是说全程没有通过 CPU 来搬运数据，所有的数据都是通过 DMA 来进行传输的。

零拷贝技术的文件传输方式相比传统文件传输的方式，减少了 2 次上下文切换和数据拷贝次数，只需要 2 次上下文切换和数据拷贝次数，就可以完成文件的传输，而且 2 次的数据拷贝过程，都不需要通过 CPU，2 次都是由 DMA 来搬运。


1. mmap + write 

mmap() 系统调用函数会直接把内核缓冲区里的数据「映射」到用户空间，这样，操作系统内核与用户空间就不需要再进行任何的数据拷贝操作。

write负责将用户缓冲区的数据拷贝到内核的 socket 的缓冲区里

但这还不是最理想的零拷贝，因为仍然需要通过 CPU 把内核缓冲区的数据拷贝到 socket 缓冲区里，而且仍然需要 4 次上下文切换，因为系统调用还是 2 次。



2. 如果网卡支持 SG-DMA（The Scatter-Gather Direct Memory Access）技术，sendfile  

sendfile() 可以直接把内核缓冲区里的数据拷贝到内核socket 缓冲区里，不再拷贝到用户态，这样就只有 2 次上下文切换，和 3 次数据拷贝。

这就是所谓的零拷贝（Zero-copy）技术，因为我们没有在内存层面去拷贝数据，也就是说全程没有通过 CPU 来搬运数据，所有的数据都是通过 DMA 来进行传输的。



# 可执行文件运行的过程

运行可执行文件后，可执行文件就会加载进内存中，成为一个进程，运行起来。
可执行文件里的机器码也会被加载到内存中，它就像是一张列满todo list的清单，而CPU就对照着这张清单，一行行的执行上面的机器码。从效果上来看，进程就动起来了。
对CPU来说，它执行到某个特定的编码数值，就会执行特定的操作。比如计算2+3，其实就是通过总线把数据2和3从内存里读入，然后放到寄存器上，再用加法器相加这两个数值并将结果放入到寄存器里，最后将这个数值回写到内存中，以此循环往复，一行行执行机器码直到退出。


# 32位系统能安8G内存条嘛？

CPU位数主要指的是寄存器的位宽
32位CPU只能装32位的系统和软件，且能计算int64，int32的数值。内存寻址范围是4G。  
32位CPU和操作系统的总线寻址能力为2的32次方，也就是4G，哪怕装了8G的内存，真正能被用到的其实只有4g，多少有点浪费。


# IO密集型机器与CPU密集型机器  

## CPU密集型

CPU密集型也叫计算密集型，指的是系统的硬盘、内存性能相对CPU要好很多，此时，系统运作CPU读写IO(硬盘/内存)时，IO可以在很短的时间内完成，而CPU还有许多运算要处理，因此，CPU负载很高。

CPU密集表示该任务需要大量的运算，而没有阻塞，CPU一直全速运行。CPU密集任务只有在真正的多核CPU上才可能得到加速（通过多线程），而在单核CPU上，无论你开几个模拟的多线程该任务都不可能得到加速，因为CPU总的运算能力就只有这么多。

## IO密集型

IO密集型指的是系统的CPU性能相对硬盘、内存要好很多，此时，系统运作，大部分的状况是CPU在等IO (硬盘/内存) 的读写操作，因此，CPU负载并不高。

密集型的程序一般在达到性能极限时，CPU占用率仍然较低。这可能是因为任务本身需要大量I/O操作，而程序的逻辑做得不是很好，没有充分利用处理器能力。

CPU 使用率较低，程序中会存在大量的 I/O 操作占用时间，导致线程空余时间很多，通常就需要开CPU核心数数倍的线程。

IO密集型核心线程数 = CPU核数 / （1-阻塞系数）。


## 总结

当线程等待时间所占比例越高，需要越多线程，启用其他线程继续使用CPU，以此提高CPU的利用率；
当线程CPU时间所占比例越高，需要越少的线程，通常线程数和CPU核数一致即可，这一类型在开发中主要出现在一些计算业务频繁的逻辑中。



# 进程间通信方式

1. 管道

管道分为匿名管道和命名管道，匿名管道由pipe系统调用创建。创建后会有两个文件句柄，一个用于读，一个用于写。匿名管道一般用于父子进程间的通信,管道是单向通信的，要实现进程之间的双向通信需要创建两个管道。命名管道由mkfifo创建，命名管道就是FIFO，管道是先进先出的通讯方式。FIFO是一种先进先出的队列。它类似于一个管道，只允许数据的单向流动。其与匿名管道的一个重要区别是它提供了一个文件路径名与之关联，任何进程只要能访问该文件就能实现进程间的相互通信。容量有限，速度慢。

2. 消息队列

消息队列是消息的连接表，存放在内核中并由消息队列标识符标识这种通信机制传递的数据具有某种结构，而不是简单的字节流。消息队列是用于两个进程之间的通讯，首先在一个进程中创建一个消息队列，然后再往消息队列中写数据，而另一个进程则从那个消息队列中取数据。需要注意的是，消息队列是用创建文件的方式建立的，如果一个进程向某个消息队列中写入了数据之后，另一个进程并没有取出数据，即使向消息队列中写数据的进程已经结束，保存在消息队列中的数据并没有消失。

3. 消息队列

信号量不能传递复杂消息，只能用来同步

4. 共享内存

只要首先创建一个共享内存区，两个进程只要按照进程A用户空间-共享内存-进程B用户空间的步骤就可以对共享内存区中的数据进行读写。
其中共享内存的效率最高，原因是共享内存的数据拷贝只有两次，即进程A的内存空间到共享内存，共享内存到进程B的内存空间。管道/消息队列的效率较低，原因是数据拷贝有四次。以消息队列为例，首先是进程A用户内存空间数据拷贝到A进程内核缓冲区，内核缓冲区数据拷贝到消息队列，进程B需要将消息队列数据拷贝到B进程内核缓冲区，最后将B进程内核缓冲区的数据拷贝到进程B的用户内存空间。

5. socket套接字

套接字也是一种进程间通信机制，与其他通信机制不同的是，它可用于不同机器间的进程通信。

unix domain socket  用于本机间进程的通信


  
# 虚拟内存

虚存是操作系统为了更高效的使用物理内存提出的概念，应用程序操作的地址是虚存的地址(对应地址空间的概念)，内核提供将虚存地址翻译为物理内存地址的功能。
虚存是对物理内存的抽象，虚存使用lru 的机制将物理内存中不经常使用的部分写入磁盘，通过这种方式来扩展系统的可用内存。当系统需要访问写入磁盘的部分时，系统会触发一个缺页异常将写入磁盘的部分写回物理内存。


# 交换空间

交换空间对应着虚存中的用来临时存储物理内存中内容的磁盘空间。当系统的物理内存不够时，我们才使用交换空间。

# 进程线程资源

## 同一进程下线程共享的资源

1. 堆  由于堆是在进程空间中开辟出来的，所以它是理所当然地被共享的；因此new出来的都是共享的（16位平台上分全局堆和局部堆，局部堆是独享的）
2. 全局变量 它是与具体某一函数无关的，所以也与特定线程无关；因此也是共享的
3. 静态变量 虽然对于局部变量来说，它在代码中是“放”在某一函数中的，但是其存放位置和全局变量一样，存于堆中开辟的.bss和.data段，是共享的
4. 文件等公用资源  这个是共享的，使用这些公共资源的线程必须同步。Win32 提供了几种同步资源的方式，包括信号、临界区、事件和互斥体。

## 线程独享的资源

1. 栈
2. 寄存器内容





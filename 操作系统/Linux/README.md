# Command

- 查看端口号对应的进程: lsof -i : 50506   
- 查找文件: find path -type d -name "xxx" 
	-type 指定类型    
	-name 文件名  
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



- 查看网络接口属性: ifconfig
- 查看路由表: route -n
- netstat -lntp # 查看所有监听端口
- netstat -antp # 查看所有已经建立的连接
- netstat -s # 查看网络统计信息


- 实时显示进程状态: top



- 查看定时任务: crontab -l

- 程序后台运行： nohup   

- 终止进程： ctrl + c  
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







# Command

- 查看端口号对应的进程: lsof -i : 50506   
- 查找文件: find path -type d -name "xxx" 
	-d 指定类型    
	-name 文件名  
- 远程传输: scp (-r) src dst    

- 本地公钥复制至远程  ssh-copy-id dst 


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

   





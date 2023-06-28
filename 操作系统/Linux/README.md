# Command

- 查看端口号对应的进程: lsof -i : 50506   
- 查找文件: find path -type d -name "xxx" -d指定类型  
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


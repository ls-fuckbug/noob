# 记一次数据库被攻击

在阿里云上搭建的mysql服务器，对外端口号为3306，由于采用了弱密码（123456） 导致数据库表内容被强制更改为恶意JS代码（重定向至色情网站）。

因此，一定要避免使用弱密码，安全无小事。



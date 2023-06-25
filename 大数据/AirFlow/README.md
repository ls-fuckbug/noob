# document

- AirFlow中文文档： https://airflow.apachecn.org/#/zh/start  



# Command  

- 启动： nohup airflow scheduler >>airflow-scheduler.log 2>& 1 &  
- 退出： exit  
- 重置数据库: airflow resetdb  
- 初始化数据库: airflow initdb  
- 启动web服务:  nohup airflow webserver -p 8080 &  
 


# 坑
- airflow一定要用绝对路径，禁止使用~/



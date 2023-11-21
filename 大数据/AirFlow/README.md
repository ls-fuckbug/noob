# document

- AirFlow中文文档： https://airflow.apachecn.org/#/zh/start  

- 概念

	Airflow 是一个工作流调度器（Workflow Schedule）和管理程序，主要用于开发和维护数据管道。这些任务以有向无环图 (DAG) 的形式表示，该图进一步包含了一系列相互关联的任务。



# Command  

- 启动scheduler： nohup airflow scheduler >>airflow-scheduler.log 2>& 1 &  
- 退出： exit  
- 重置数据库: airflow resetdb  
- 初始化数据库: airflow initdb  
- 启动web服务:  nohup airflow webserver -p 8080 &  
 


# 坑
- airflow一定要用绝对路径，避免使用相对路径。



# 有用的SQL命令

## CONCAT
在 MySQL 中，CONCAT 函数用于将多个字符串连接在一起。它接受两个或多个参数，并返回这些参数连接后的字符串。
SELECT CONCAT('My name is ', first_name, ' ', last_name) AS full_name FROM users;  

## EXPLAIN
EXPLAIN 是 MySQL 数据库提供的一个关键字，用于分析和优化查询语句的执行计划。通过 EXPLAIN，您可以获取有关查询的详细信息，了解 MySQL 在执行查询时所采取的操作和优化策略。

要使用 EXPLAIN，只需在查询语句前加上 EXPLAIN 关键字，然后执行该语句。MySQL 将返回一个描述查询执行计划的结果集，其中包含以下重要的列：

id: 表示查询中每个操作的标识符，主要用于连接操作。  
select_type: 描述查询的类型，例如简单查询、联接查询、子查询等。  
table: 指示查询涉及的表。  
type: 表示每个表的访问方式，例如全表扫描、索引扫描、范围扫描等。  
possible_keys: 表示可能用于查询的索引。  
key: 表示实际选择的索引。  
key_len: 表示索引使用的字节数。  
ref: 表示索引的比较列。  
rows: 表示查询扫描的行数估计值。  
Extra: 提供其他与查询相关的附加信息，如排序操作、临时表使用等。  



# 坑 

## 大小写

MySQL在Linux下数据库名、表名、列名、别名大小写规则是这样的：
1、数据库名与表名是严格区分大小写的；  
2、表的别名是严格区分大小写的；  
3、列名与列的别名在所有的情况下均是忽略大小写的；  
4、字段内容默认情况下是大小写不敏感的。  

InnoDB在创建表时可以设置 COLLATE utf8mb4_bin指定大小写敏感  
`show collation where charset = 'utf8mb4';`
ci结尾为大小写不敏感，bin结尾的是大小写敏感  



# 基础知识 









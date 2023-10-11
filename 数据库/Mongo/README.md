# 概念

MongoDB是一个面向文档的数据库，它以类似JSON格式的BSON（Binary JSON）文档来存储数据。每个文档可以具有不同的结构，这使得MongoDB非常灵活且适用于存储各种类型的数据，而无需事先定义数据模式。这种非常自由的数据模型使得MongoDB在开发过程中更加灵活，容易进行迭代和快速原型设计。

与传统的关系型SQL数据库相比，MongoDB具有以下几个显著的区别：

数据模型：MongoDB是面向文档的数据库，数据以文档的形式存储，每个文档可以包含嵌套的子文档和数组。而传统SQL数据库使用表格来存储数据，需要定义表格结构和关系。  
架构设计：在MongoDB中，设计数据库结构相对简单，不需要像传统SQL数据库那样进行规范化和拆分。这样可以减少复杂的JOIN操作，并提高查询性能。  
数据查询：MongoDB的查询语言非常强大，支持复杂的查询和聚合操作，同时具有灵活的索引和查询优化。而在传统SQL数据库中，查询需要使用SQL语言，可能需要多个JOIN操作。  
扩展性：MongoDB天生支持横向扩展，也就是说可以通过添加更多的服务器来分布数据和负载。这使得MongoDB在大规模应用和处理海量数据时表现出色。  



# Command

显示所有数据库： show dbs  
切换数据库： use xxx  
显示所有集合： show collections  
文档增删改查： 
```
插入文档： db.<collection_name>.insertOne(<document>)
查询文档： db.<collection_name>.find(<query>)   
		  query: { name: "John" }  
更新文档： db.<collection_name>.updateOne(<filter>, <update>) 
删除文档： db.<collection_name>.deleteOne(<filter>) 


```
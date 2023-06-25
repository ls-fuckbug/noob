# 概念

Elasticsearch是一个开源的搜索引擎，分布式的实时文件存储，可以处理PB级结构化或非结构化数据， 我们可以通过简单的RESTful API来完成各种操作。

node : 服务实例  
index :  类似mysql数据库  
type :  类似mysql表， 从6.0.0开始单个索引中只能有一个类型，7.0.0以后将将不建议使用，8.0.0 以后完全不支持。
document : 类似mysql记录  

弃用type的原因： 
我们虽然可以通俗的去理解Index比作 SQL 的 Database，Type比作SQL的Table。但这并不准确，因为如果在SQL中,Table 之前相互独立，同名的字段在两个表中毫无关系。  
但是在ES中，同一个Index 下不同的 Type 如果有同名的字段，他们会被 Luecence 当作同一个字段 ，并且他们的定义必须相同。所以我觉得Index现在更像一个表，  
而Type字段并没有多少意义。目前Type已经被Deprecated，在7.0开始，一个索引只能建一个Type为_doc  



# Command

查看索引对应的mapping
```
GET   /xxxx/_mapping

```

查看索引定义信息
```
GET /_cat/indices?v
```

查询文档

```
GET /example/_doc/37191ffa-31eb-4c1c-8d77-821867e6e41c


GET /{索引名}/_search
{
        "from" : 0,  // 返回搜索结果的开始位置
        "size" : 10, // 分页大小，一次返回多少数据
        "_source" :[ ...需要返回的字段数组... ],
        "query" : { ...query子句... },
        "aggs" : { ..aggs子句..  },
        "sort" : { ..sort子句..  }
}


// 查询所有
GET xxx/_search
{
    "query":
    {
        "match_all":
        {}
    }
}


// 查询总数
GET xxx/_count
{
    "query":
    {
        "match_all":
        {}
    }
}


// 查询按字符串搜索
GET /xxx/_search?q=last_name:Smith


```

创建文档

```
POST /example/_doc
{
}
```

删除文档

```
DELETE /example/_doc/2YztyYMB0nNG5LcEnAb8
```







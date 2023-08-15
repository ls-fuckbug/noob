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


# es数据类型


## string类型

    text: 会分词  
    keyworld: 不会分词  

## date时间类型  

    和mysql类似，但可以控制时间的format

## 复杂类型        

    array: 在Elasticsearch中，数组不需要专用的字段数据类型。默认情况下，任何字段都可以包含零个或多个值，但是，数组中的所有值都必须具有相同的数据类型。  
    object: 即对象，需要注意的是，List<object>中的 object不允许彼此独立地索引查询。
    nested: 需要建立对象数组的索引并保持数组中每个对象的独立性，则应使用nested数据类型而不是 object数据类型。在内部，嵌套对象索引阵列作为一个单独的隐藏文档中的每个对象，这意味着每个嵌套的对象可以被独立的查询。  



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
// 根据id
GET /example/_doc/37191ffa-31eb-4c1c-8d77-821867e6e41c

// 根据query
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
        "match_all":{}
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

// term查询， 不分词直接匹配词条
GET es_user/_search
{
  "query": {
    "term": {
      "address": {
        "value": "杭州好地方"
      }
    }
  }
}

// prefix查询
GET es_user/_search
{
  "query": {
    "prefix": {
      "address": {
        "value": "杭"
      }
    }
  }
}

// wildcard查询，不分词通配符的方式匹配词条。
GET es_user/_search    
{
  "query": {
    "wildcard": {
      "address": {
        "value": "杭州*"
      }
    }
  }
}

// range查询
POST es_user/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 32,
        "lte": 36
      }
    }
  }
}

// 复合查询
must、must not、should

// filter查询， filter不影响文档的评分，不会改变文档的相关性排序。


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







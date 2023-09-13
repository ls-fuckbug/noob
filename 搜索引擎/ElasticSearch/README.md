# 概念

Elasticsearch是一个开源的搜索引擎，分布式的实时文件存储，可以处理PB级结构化或非结构化数据， 我们可以通过简单的RESTful API来完成各种操作。（通过隐藏 Lucene 的复杂性，取而代之的提供一套简单一致的 RESTful API）  

- cluster: 一个集群由一个唯一的名字标识，默认为“elasticsearch”。集群名称非常重要，具有相同集群名的节点才会组成一个集群。集群名称可以在配置文件中指定。

- node : 服务实例, 存储集群的数据，参与集群的索引和搜索功能。像集群有名字，节点也有自己的名称，默认在启动时会以一个随机的UUID的前七个字符作为节点的名字，你可以为其指定任意的名字。通过集群名在网络中发现同伴组成集群。一个节点也可是集群。

- index :  类似mysql数据库, 一个索引是一个文档的集合（等同于solr中的集合）。每个索引有唯一的名字，通过这个名字来操作它。一个集群中可以有任意多个索引。  

- type :  类似mysql表， 从6.0.0开始单个索引中只能有一个类型，7.0.0以后将将不建议使用，8.0.0 以后完全不支持。

- document : 类似mysql记录, 被索引的一条数据，索引的基本信息单元，以JSON格式来表示。

- shard: 分片， 在创建一个索引时可以指定分成多少个分片来存储。每个分片本身也是一个功能完善且独立的“索引”，可以被放置在集群的任意节点上。

- replication: 备份，一个分片可以有多个备份。

- mapping: 类似mysql的表结构schema

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


# es索引管理  

## 禁止自动创建索引  

可以通过在 config/elasticsearch.yml 的每个节点下添加下面的配置：
action.auto_create_index: false

## 索引格式

settings: 用来设置分片,副本等配置信息  
mappings: 字段映射，类型等  


# es复合查询详解 

## bool查询  

- must： 必须匹配。贡献算分  
- must_not：过滤子句，必须不能匹配，但不贡献算分
- should： 选择性匹配，至少满足一条。贡献算分
- filter： 过滤子句，必须匹配，但不贡献算分

## boosting query 提高查询  

不同于bool查询，bool查询中只要一个子查询条件不匹配那么搜索的数据就不会出现。而boosting query则是降低显示的权重/优先级（即score)。


## 固定分数查询

查询某个条件时，固定的返回指定的score；显然当不需要计算score时，只需要filter条件即可，因为filter context忽略score。

## dis_max 最佳匹配查询  

分离最大化查询（Disjunction Max Query）指的是： 将任何与任一查询匹配的文档作为结果返回，但只将最佳匹配的评分作为查询的评分结果返回 。


## function_score 函数查询  

简而言之就是用自定义function的方式来计算_score。


# es全文搜索详解  

## Match类型

  1. 查询字段类型
  2. 分析查询字符串  
  3. 查找匹配文档  
  4. 为文档打分  

## Query String类型  

  此查询使用语法根据运算符（例如AND或）来解析和拆分提供的查询字符串NOT。然后查询在返回匹配的文档之前独立分析每个拆分的文本。


# es Term查询 

## ids查询， 即对id查找

```
GET /test-dsl-term-level/_search
{
  "query": {
    "ids": {
      "values": [3, 1]
    }
  }
}

```

## prefix查询

```
GET /test-dsl-term-level/_search
{
  "query": {
    "prefix": {
      "name": {
        "value": "Jan"
      }
    }
  }
}
```

## term 分词匹配

```
GET /test-dsl-term-level/_search
{
  "query": {
    "term": {
      "programming_languages": "php"
    }
  }
}

```

## wildcard 通配符查询

```
GET /test-dsl-term-level/_search
{
  "query": {
    "wildcard": {
      "name": {
        "value": "D*ai",
        "boost": 1.0,
        "rewrite": "constant_score"
      }
    }
  }
}
```

## range 范围查询  

```
GET /test-dsl-term-level/_search
{
  "query": {
    "range": {
      "required_matches": {
        "gte": 3,
        "lte": 4
      }
    }
  }
}

```

## regexp 正则查询

```
GET /test-dsl-term-level/_search
{
  "query": {
    "regexp": {
      "name": {
        "value": "Ja.*",
        "case_insensitive": true
      }
    }
  }
}

```

# es搜索过程

1. 请求分发到集群中的节点， 此时被选中的节点成为当前请求的协调者，由它决定根据索引信息请求会被路由到哪些节点。  
2. query被转换为lucene query， 然后在所有的segement中进行计算。  
3. 搜索结束之后，结果会沿着下行的路径向上逐层返回。  






# Command

查看索引对应的mapping
```
GET   /xxxx/_mapping

```

查看索引定义信息
```
GET /_cat/indices?v
```

创建文档

```
POST /example/_doc
{
  "name": "superlittlesam"
}
```

删除文档

```
DELETE /example/_doc/2YztyYMB0nNG5LcEnAb8
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

# 查询返回的字段解释

took – Elasticsearch运行查询所花费的时间（以毫秒为单位）  
timed_out –搜索请求是否超时  
_shards - 搜索了多少个碎片，以及成功，失败或跳过了多少个碎片的细目分类。  
max_score – 找到的最相关文档的分数  
hits.total.value - 找到了多少个匹配的文档  
hits.hits._score - 文档的相关性得分（使用match_all时不适用） 




# 友情链接

[es知识体系](https://pdai.tech/md/db/nosql-es/elasticsearch.html)  






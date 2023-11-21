# 概念

Elasticsearch是一个开源的搜索引擎，分布式的实时文件存储，可以处理PB级结构化或非结构化数据， 我们可以通过简单的RESTful API来完成各种操作。（通过隐藏 Lucene 的复杂性，取而代之的提供一套简单一致的 RESTful API）  

- cluster: 一个集群由一个唯一的名字标识，默认为“elasticsearch”。集群名称非常重要，具有相同集群名的节点才会组成一个集群。集群名称可以在配置文件中指定。

- node : 服务实例, 存储集群的数据，参与集群的索引和搜索功能。像集群有名字，节点也有自己的名称，默认在启动时会以一个随机的UUID的前七个字符作为节点的名字，你可以为其指定任意的名字。通过集群名在网络中发现同伴组成集群。一个节点也可是集群。

- index :  类似mysql数据库, 一个索引是一个文档的集合（等同于solr中的集合）。每个索引有唯一的名字，通过这个名字来操作它。一个集群中可以有任意多个索引。  

- type :  类似mysql表， 从6.0.0开始单个索引中只能有一个类型，7.0.0以后将将不建议使用，8.0.0 以后完全不支持。

- document : 类似mysql记录, 被索引的一条数据，索引的基本信息单元，以JSON格式来表示。

- shard: 分片， 在创建一个索引时可以指定分成多少个分片来存储。每个分片本身也是一个功能完善且独立的“索引”，可以被放置在集群的任意节点上。

- segment: 段

在 Elasticsearch 中，最基本的数据存储单位是 shard。 但是，通过 Lucene 镜头看，情况会有所不同。 在这里，每个 Elasticsearch 分片都是一个 Lucene 索引 (index)，每个 Lucene 索引都包含几个 Lucene segments。 一个 Segment 包含映射到文档里的所有术语（terms) 一个反向索引 (inverted index)。

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

    object: 即对象，需要注意的是，List<object>中的 object不允许彼此独立地索引查询。当 JSON 对象被 Lucene 扁平化后， 就失去了object内部结构的关联关系。

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

  此查询使用语法根据运算符（例如AND或NOT）来解析和拆分提供的查询字符串。然后查询在返回匹配的文档之前独立分析每个拆分的文本。


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

```
"took" : Elasticsearch运行查询所花费的时间（以毫秒为单位）  
"timed_out" : 搜索请求是否超时  
"_shards" :  搜索了多少个碎片，以及成功，失败或跳过了多少个碎片的细目分类。  
"max_score" : 找到的最相关文档的分数  
"hits.total.value" : 找到了多少个匹配的文档  
"hits.hits._score" : 文档的相关性得分（使用match_all时不适用）  
```

# 为什么要使用ES

系统中的数据，随着业务的发展，时间的推移，将会非常多，而业务中往往采用模糊查询进行数据的搜索，而模糊查询会导致查询引擎放弃索引，导致系统查询数据时都是全表扫描，在百万级别的数据库中，查询效率是非常低下的，而我们使用ES做一个全文索引，将经常查询的系统功能的某些字段，比如说电商系统的商品表中商品名，描述、价格还有id这些字段我们放入ES索引库里，可以提高查询速度。



# es搜索文档的过程

1. 请求分发到集群中的节点， 此时被选中的节点成为当前请求的协调者，由它决定根据索引信息请求会被路由到哪些节点。  
2. query被转换为lucene query， 然后在所有的segement中进行计算。  
3. 在初始查询阶段时，查询会广播到索引中每一个分片拷贝（主分片或者副本分片）。 每个分片在本地执行搜索并构建一个匹配文档的大小为 from + size 的优先队列。PS：在搜索的时候是会查询Filesystem Cache的，但是有部分数据还在Memory Buffer，所以搜索是近实时的。

4. 每个分片返回各自优先队列中 所有文档的 ID 和排序值 给协调节点，它合并这些值到自己的优先队列中来产生一个全局排序后的结果列表。

5. 接下来就是取回阶段，协调节点辨别出哪些文档需要被取回并向相关的分片提交多个 GET 请求。每个分片加载并丰富文档，如果有需要的话，接着返回文档给协调节点。一旦所有的文档都被取回了，协调节点返回结果给客户端。



# es索引文档的流程

1. 协调节点默认使用文档ID参与计算（也支持通过routing），以便为路由提供合适的分片：

shard = hash(document_id) % (num_of_primary_shards)

2. 当分片所在的节点接收到来自协调节点的请求后，会将请求写入到Memory Buffer，然后定时（默认是每隔1秒）写入到Filesystem Cache，这个从Memory Buffer到Filesystem Cache的过程就叫做refresh；

3. 当然在某些情况下，存在Momery Buffer和Filesystem Cache的数据可能会丢失，ES是通过translog的机制来保证数据的可靠性的。其实现机制是接收到请求后，同时也会写入到translog中，当Filesystem cache中的数据写入到磁盘中时，才会清除掉，这个过程叫做flush；

4. 在flush过程中，内存中的缓冲将被清除，内容被写入一个新段，段的fsync将创建一个新的提交点，并将内容刷新到磁盘，旧的translog将被删除并开始一个新的translog。

5. flush触发的时机是定时触发（默认30分钟）或者translog变得太大（默认为512M）时；


# es删除和更新文档的流程

1. 删除和更新也都是写操作，但是Elasticsearch中的文档是不可变的，因此不能被删除或者改动以展示其变更；

2. 磁盘上的每个段都有一个相应的.del文件。当删除请求发送后，文档并没有真的被删除，而是在.del文件中被标记为删除。该文档依然能匹配查询，但是会在结果中被过滤掉。当段合并时，在.del文件中被标记为删除的文档将不会被写入新段。

3. 在新的文档被创建时，Elasticsearch会为该文档指定一个版本号，当执行更新时，旧版本的文档在.del文件中被标记为删除，新版本的文档被索引到一个新段。旧版本的文档依然能匹配查询，但是会在结果中被过滤掉。


# 提升文档索引速度的方案

1. 使用批量请求并调整其大小：每次批量数据 5–15 MB 大是个不错的起始点。

2. 存储：使用 SSD

3. 如果你的搜索结果不需要近实时的准确度，考虑把每个索引的index.refresh_interval 改到30s。

4. 如果你在做大批量导入，考虑通过设置index.number_of_replicas: 0 关闭副本。


# 并发情况下如何保证读写一致

- 可以通过版本号使用乐观并发控制，以确保新版本不会被旧版本覆盖，由应用层来处理具体的冲突；

- 另外对于写操作，一致性级别支持quorum/one/all，默认为quorum，即只有当大多数分片可用时才允许写操作。但即使大多数可用，也可能存在因为网络等原因导致写入副本失败，这样该副本被认为故障，分片将会在一个不同的节点上重建。

- 对于读操作，可以设置replication为sync(默认)，这使得操作在主分片和副本分片都完成后才会返回；如果设置replication为async时，也可以通过设置搜索请求参数_preference为primary来查询主分片，确保文档是最新版本。


# es分片shard

一个 分片 是一个底层的 工作单元 ，它仅保存了全部数据中的一部分，一个分片是一个 Lucene 的实例，它本身就是一个完整的搜索引擎。我们的文档被存储和索引到分片内，但是应用程序是直接与索引而不是与分片进行交互。
Elasticsearch 是利用分片将数据分发到集群内各处的。分片是数据的容器，文档保存在分片内，分片又被分配到集群内的各个节点里。当你的集群规模扩大或者缩小时， Elasticsearch 会自动的在各节点中迁移分片，使得数据仍然均匀分布在集群里。

- 主分片 primary shard：
在索引建立的时候就已经确定了主分片数，但是副本分片数可以随时修改，索引内任意一个文档都归属于一个主分片，所以主分片的数目决定着索引能够保存的最大数据量。 默认主分片数量为5.

- 副分片 replica shard：
一个副本分片只是一个主分片的拷贝。副本分片作为硬件故障时保护数据不丢失的冗余备份，并为搜索和返回文档等读操作提供服务。
当主分片异常的时候，可以从副本分片中选择一个作为主分片，提高可用性。同时，查询可以在副本分片进行，减轻主分片的压力，提高性能。另外副本分片必须和主分片部署在不同的节点上，如果集群中只有一个节点，则副本分片将不会被分配，此时集群健康状态为yellow，存在丢失数据的风险。


## 分片基本策略

- ES使用数据分片（shard）来提高服务的可用性，将数据分散保存在不同的节点上以降低当单个节点发生故障时对数据完整性的影响，同时使用副本（repiica）来保证数据的完整性。关于分片的默认分配策略，在7.x之前，默认5个primary shard，每5个primary shard默认分配一个replica，即5主1副，而7.x之后，默认1主1副  

- ES在分配单个索引的分片时会将每个分片尽可能分配到更多的节点上。但是，实际情况取决于集群拥有的分片和索引的数量以及它们的大小，不一定总是能均匀地分布。

- primary只能在索引创建时配置数量，而replica可以在任何时间分配，并且primary支持读和写操作，而replica只支持客户端的读取操作，数据由es自动管理，从primary同步。  

- ES不允许Primary和它的Replica放在同一个节点中，并且同一个节点不接受完全相同的两个Replica
- 同一个节点允许多个索引的分片同时存在。



## 分片数量多少

- 避免分片过多：大多数搜索会命中多个分片。每个分片在单个 CPU 线程上运行搜索。虽然分片可以运行多个并发搜索，但跨大量分片的搜索会耗尽节点的搜索线程池。这会导致低吞吐量和缓慢的搜索速度。 
- 分片越少越好：每个分片都使用内存和 CPU 资源。在大多数情况下，一小组大分片比许多小分片使用更少的资源。 

## 分片大小决策

- 分片的合理容量：10GB-50GB。虽然不是硬性限制，但 10GB 到 50GB 之间的分片往往效果很好。根据网络和用例，也许可以使用更大的分片。在索引的生命周期管理中，一般设置50GB为单个索引的最大阈值。 
- 堆内存容量和分片数量的关联：小于20分片/每GB堆内存，一个节点可以容纳的分片数量与节点的堆内存成正比。例如，一个拥有 30GB 堆内存的节点最多应该有 600 个分片。如果节点超过每 GB 20 个分片，考虑添加另一个节点。
 
## 分片平衡策略 shard rebalance

当集群在每个节点上具有相同数量的分片而没有集中在任何节点上的任何索引的分片时，集群是平衡的。Elasticsearch 运行一个称为rebalancing 的自动过程，它在集群中的节点之间移动分片以改善其平衡。

## 数据路由

当客户端发起创建document的时候，es需要确定这个document放在该index哪个shard上。这个过程就是数据路由。


路由算法：shard = hash(routing) % number_of_primary_shards

routing值默认是document的ID值，也可以自行指定。

如果number_of_primary_shards在查询的时候取余发生的变化，无法获取到该数据

注意：索引的主分片数量定义好后，不能被修改


# es主从

ES的集群是随机指定一个ES服务作为Master节点，当指定的Master主节点挂了，会通过选举的方式将一台从节点的Salve Es服务升级成Master节点

一个运行中的Elasticsearch实例称为一个节点，它是一个逻辑上独立的服务，可以存储数据，是ES集群的一部分。ES集群由一个或者多个拥有相同cluster.name配置的节点组成，它们共同承担数据和负载的压力。ES集群中的节点有三种不同的类型：


- 主节点：负责管理集群范围内的所有变更，例如增加、删除索引，或者增加、删除节点等，并决定哪些分片分配给相关的节点、追踪集群中节点的状态等。主节点并不需要涉及到文档级别的变更和搜索等操作，可以通过属性node.master进行设置。  
- 数据节点：存储数据和其对应的倒排索引，同时对数据进行增删改查和聚合等操作。默认每一个节点都是数据节点（包括主节点），可以通过node.data属性进行设置。通常随着集群的扩大，需要增加更多的数据节点来提高性能和可用性。  
- 协调节点：如果node.master和node.data属性均为false，则此节点称为协调节点，用来响应客户请求，均衡每个节点的负载。  

## 主节点选举

当主节点发生问题的时候，现有的节点会通过Ping的方式重新选举一个新的主节点。

1. 选举开始时，从各节点认为的Master中选择，按照ID的字典顺序排序，取第一个
2. 如果各节点没有认为的Master，则从所有节点中选择，规则同上
3. 如果节点数达不到discovery.zen.minimum_master_nodes设定的最小值，则循环上述过程直到节点数足够开始选举
4. 如果当前节点是Master，则等待节点数达到minimum_master_nodes，开始提供服务
5. 为避免网络异常，配置discovery.zen.ping_timeout设置超时时间

master节点的职责主要包括集群、节点和索引的管理，不负责文档级别的管理；data节点可以关闭http功能。

### 脑裂产生的原因

- 网络问题：集群间的网络延迟导致一些节点访问不到master，认为master挂掉了从而选举出新的master，并对master上的分片和副本标红，分配新的主分片

- 节点负载：主节点的角色既为master又为data，访问量较大时可能会导致ES停止响应造成大面积延迟，此时其他节点得不到主节点的响应认为主节点挂掉了，会重新选取主节点。


### 脑裂的解决方法

- 减少误判： discovery.zen.ping_timeout节点状态的响应时间，默认为3s，可以适当调大，如果master在该响应时间的范围内没有做出响应应答，判断该节点已经挂掉了。调大参数（如6s，discovery.zen.ping_timeout:6），可适当减少误判。

- 选举触发: discovery.zen.minimum_master_nodes:1  

该参数是用于控制选举行为发生的最小集群主节点数量。当备选主节点的个数大于等于该参数的值， 且备选主节点中有该参数个节点认为主节点挂了，进行选举。官方建议为（n/2）+1，n为主节点个数 （即有资格成为主节点的节点个数）


需要注意的是Elasticsearch中支持任意数目的集群，没有限定节点数必须是奇数，而是通过一定的规则来约定。但是在分布式系统中，很容易出现脑裂，解决方案是设置一个Quorum值，并且要求可用节点数超过Quorum才能对外提供服务。


## 故障检测

Elasticsearch节点的故障检测有两种方式：第一种是Master节点到所有其它节点，证明它们还活着；另一种是每个节点Ping主节点进行验证，当主节点故障时会启动主节点重新选举过程。

## 集群扩展

每个索引的primary shard数量在索引创建的时候已经确定，如果想调整主分片需要重建索引，但是replica shard是可以动态调整的。

集群扩容时需要注意的是：

- 同一个index的primary shard和replica shard不能分配到一个节点上
- 尽量保证shard均匀分布在不同的节点上达到负载均衡



# 倒排索引

单词词典 + 倒排列表

es有一个file system cache ,第一次查询数据时，其实是去磁盘读，然后刷进缓存的，所以缓存要存热点数据，内存容量有限，，我们可以先预热一下，比如后台有一个刷到缓存的数据进程，，这样每次查询都会去缓存中查询。。


# es flush和refresh的区别

## 一条document写入到es的整体流程

1. 数据写入buffer缓冲和translog日志文件中。
当你写一条数据document的时候，一方面写入到mem buffer缓冲中，一方面同时写入到translog日志文件中。

2. buffer满了或者每隔1秒(可配)，refresh将mem buffer中的数据生成index segment文件并写入os cache，此时index segment可被打开以供search查询读取，这样文档就可以被搜索到了（注意，此时文档还没有写到磁盘上）；然后清空mem buffer供后续使用。可见，refresh实现的是文档从内存移到文件系统缓存的过程。

3. 重复上两个步骤，新的segment不断添加到os cache，mem buffer不断被清空，而translog的数据不断增加，随着时间的推移，translog文件会越来越大。

4. 当translog长度达到一定程度的时候，会触发flush操作，否则默认每隔30分钟也会定时flush，其主要过程：

  4.1 执行refresh操作将mem buffer中的数据写入到新的segment并写入os cache，然后打开本segment以供search使用，最后再次清空mem buffer。

  4.2. 一个commit point被写入磁盘，这个commit point中标明所有的index segment。

  4.3. filesystem cache（os cache）中缓存的所有的index segment文件被fsync强制刷到磁盘os disk，当index segment被fsync强制刷到磁盘上以后，就会被打开，供查询使用。

  4.4. translog被清空和删除，创建一个新的translog。

## refresh

### 背景

最原始的ES版本里，必须等待fsync将segment刷入磁盘，才能将segment打开供search使用，这样的话，从一个document写入到它可以被搜索，可能会超过一分钟，主要瓶颈是在fsync实际发生磁盘IO写数据进磁盘，是很耗时的，这就不是近实时的搜索了。为此，引入refresh操作的目的是提高ES的实时性，使添加文档尽可能快的被搜索到，同时又避免频繁fsync带来性能开销

### 写入过程

当我们把一条数据写入到 Elasticsearch 中后，它并不能马上被用于搜索。新增的索引必须写入到 Segment 后才能被搜索到，因此我们把数据写入到内存缓冲区之后并不能被搜索到。新增了一条记录时，Elasticsearch 会把数据写到 translog 和 in-memory buffer (内存缓存区)中。

在此期间，该文档不能被搜索，但是我们还是可以通过 ID 使用 GET 来获得该文档。如果希望该文档能立刻被搜索，需要手动调用 refresh 操作。在 Elasticsearch 中，默认情况下 refresh 操作设置为每秒执行一次。 在此操作期间，内存中缓冲区的内容将复制到内存中新创建的 Segment 中。 

将缓存数据生成segment后刷入os cache，并被打开供搜索的过程就叫做refresh，默认每隔1秒。也就是说，每隔1秒就会将buffer中的数据写入一个新的index segment file，先写入os cache中。所以，es是近实时的，输入写入到os 

### 批量建索引时的优化

refresh 的开销比较大,我在自己环境上测试 10W 条记录的场景下 refresh 一次大概要 14ms，因此在批量构建索引时可以把 refresh 间隔设置成 -1 来临时关闭 refresh, 等到索引都提交完成之后再打开 refresh

另外当你在做批量索引时,可以考虑把副本数设置成0，因为 document 从主分片 (primary shard) 复制到从分片 (replica shard) 时,从分片也要执行相同的分析、索引和合并过程,这样的开销比较大，你可以在构建索引之后再开启副本，这样只需要把数据从主分片拷贝到从分片


## flush

当向elasticsearch发送创建document文档添加请求的时候，document数据会先进入到buffer，与此同时会将操作记录在translog之中，当发生refresh时（数据从index buffer中进入filesystem cache的过程）translog中的操作记录并不会被清除，而当数据从os cache中被写入磁盘之后才会将translog中清空。这个将os cache的索引文件(segment file)持久化到磁盘的过程就是flush，flush之后，这段translog的使命就完成了，因为segment已经写入磁盘，就算故障也可以从磁盘的segment文件中恢复。

flush的时机可能是 1.定时flush； 2.translog大小达到阈值； 3.一些重要操作; 4.指令触发。


## translog

translog记录的是已经在内存生成(segments)并存储到os cache但是还没写到磁盘的那些索引操作，此时这些新写入的数据可以被搜索到，但是当节点挂掉后这些未来得及落入磁盘的数据就会丢失，可以通过trangslog恢复。

当然translog本身也是磁盘文件，频繁的写入磁盘会带来巨大的IO开销，因此对translog的追加写入操作的同样操作的是os cache，因此也需要定时落盘（fsync）。translog落盘的时间间隔直接决定了ES的可靠性，因为宕机可能导致这个时间间隔内所有的ES操作既没有生成segment磁盘文件，又没有记录到Translog磁盘文件中，导致这期间的所有操作都丢失且无法恢复。

translog的fsync是ES在后台自动执行的，默认是每5秒钟主动进行一次translog fsync，或者当translog文件大小大于512MB主动进行一次fsync

当系统重启时会从translog中恢复之前记录的操作。
当对elasticsearch进行CRUD操作的时候，会先到translog之中进行查找，因为tranlog之中保存的是最新的数据。
translog的清除时间时进行flush操作之后（将数据从filesystem cache刷入disk之中）。


## segment

在ES中，单个倒排索引文件被称为Segment,这个Segment一旦生成就不可变更，多个Segments汇总在一起，就是一个Shards



# 如何合并es联合索引查询？

两种方式：

1. 使用skip list结构遍历

2. 使用bitset数据结构， 构建bitset然后进行AND操作


# FST

FST最重要的功能是可以实现key到value的映射，相当于HashMap。FST的查询速度比hashMap慢一点，但是内存消耗比hashMap小很多。FST在lucene大量使用:倒排索引的存储，同义词词典的存储，搜索关键字建议等等。

## es倒排索引

Elasticsearch调用lucene为msg字段都建立了一个倒排索引，i, study, elasticsearch, MySql,Redis这些叫term，而 1，2，3 就是Posting List。Posting list就是一个int的数组，存储了所有包含某个term的文档id。通过posting list这种索引方式可以很快进行查找，比如要找包含study的文档，通过对term搜索，匹配到study后随即就可以得到对应的posting list定位到具体的文档

### term Dictionary

Elasticsearch为了能快速找到某个term，将所有的term排序，二分法查找term，logN的查找效率，就像通过字典查找一样，这就是Term Dictionary。类似于传统数据库的B-Tree的，但是Term Dictionary较B-Tree的查询快。

### term Index

B-Tree通过减少磁盘寻道次数来提高查询性能，Elasticsearch也是采用同样的思路，直接通过内存查找term，不读磁盘，但是如果term太多，term dictionary也会很大，放内存不现实，于是有了Term Index，就像字典里的索引页一样，A开头的有哪些term，分别在哪页，term index其实是一颗 (trie) 前缀树，这棵树不会包含所有的term，它包含的是term的一些前缀。通过term index可以快速地定位到term dictionary的某个offset，然后从这个位置再往后顺序查找。


### 前缀树的问题

ES为了能提高查询效率，能够在内存中操作查询term，使用了前缀树term index来优化查询。

但前缀树存在一个问题，用上面的term index为例，当插入一个新的term tasted到term中时，我们可以看到ted这个字符之前就已经存在term index中，新term tasted的插入使ted出现了重复，数据的重复势必导致空间浪费。在大数据量的ES中term数以亿计，前缀树中字符重复的问题浪费的空间就特别庞大了，因此ES使用了FST数据结构来压缩优化term index数据。

### FST 有穷状态转换器

FST是将term拆成单个字母存在节点上，每个节点存一个字母，根节点不存，从根节点出发到特定节点所经过的字母就可以组成目标term，其经过路径的权重和就是该term在Term Dictionary中对应的位置

- 优点

1. 空间占用小。通过对词典中单词前缀和后缀的重复利用，压缩了存储空间，300万条数字使用FST存储 文件大小2k左右

2. 查询速度快。O(len(str))的查询时间复杂度。


# 友情链接

[es知识体系](https://pdai.tech/md/db/nosql-es/elasticsearch.html)  






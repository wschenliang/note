- @Document(indexName="test",type="article",indexStoreType="fs",shards=5,replicas=1,refreshInterval="-1")在类上加此注解可以自动创建索引名和类型，分片，副本。

- 索引库indices  indices是index的复数，代表许多的索引，

- 类型type类型是模拟mysql中的table概念，一个索引库下可以有不同类型的索引，比如商品索引，订单索引，其数据格式不同。不过这会导致索引库混乱，因此未来版本中会移除这个概念

- 文档document存入索引库原始的数据。比如每一条商品信息，就是一个文档

- 字段field文档中的属性

- 映射配置mappings字段的数据类型、属性、是否索引、是否存储等特性

- 分片shard数据拆分后的各个部分

- 副本replica每个分片的复制

- 集群Cluster ES可以作为一个独立的单个搜索服务器。不过，为了处理大型数据集，实现容错和高可用性，ES可以运行在许多互相合作的服务器上。这些服务器的集合称为集群。

- 节点node 形成集群的每个服务器称为节点

 

*注意：*
以下的test都是我的索引库名。

## 新建索引库

```
PUT /索引库名

{

  "settings": {

    "number_of_shards": 3,  分片数量

    "number_of_replicas": 1  副本数量

  }

}
```

- GET test  查看索引设置

- DELETE test 删除索引库

- HEAD test 判断索引是否存在

- GET *  查看索引索引库配置

 

## 创建索引字段

相当于mysql 的字段。

但是字段创建思考五个点：1.字段数据类型 2.是否要存储 3.是否要索引 4. 是否要分词 5.分词器种类

```
 PUT /索引库名/_mapping/类型名称

{

  "properties": {

    "字段名": {

      "type": "类型",  text、long、short、date、integer、object

      "index": true, 默认true 字段会被索引，则可以用来进行搜索

      "store": true，默认false

      "analyzer": "分词器"

    }

  }

}
```
 

**注意：**

- "store": true，默认false 在学习lucene和solr时，我们知道如果一个字段的store设置为false， 那么在文档列表中就不会有这个字段的值，用户的搜索结果中不会显示出来。 但是在Elasticsearch中，即便store设置为false，也可以搜索到结果
- 原因是Elasticsearch在创建文档索引时，会将文档中的原始数据备份，保存到一个叫做_source的属性中。
- 最终我们查询数据是从_source中来取值，所以可以通过过滤_source来选择哪些要显示，哪些不显示。

```
PUT test/_mapping/类型名称

{

  "properties": {

    "title": {

      "type": "text", (定义成text可以分词)

      "analyzer": "ik_max_word"

    },

    "images": {

      "type": "keyword", (定义成keyword不可以分词)

      "index": "false"

    },

    "price": {

      "type": "float"

    }

  }

}
```
返回
> {"acknowledged": true}

- GET /test/_mapping 查看映射关系，相当于看数据的字段怎么定义的

对于索引的字段 可以创建可以查看，就是不可以修改和删除。


## 向索引库添加数据

```

POST /索引库名/类型名

{

    "key":"value"

}
```

新增时设置id 不加id则es会默认给你指定个id

```
POST /索引库名/类型/id值

{

   "key":"value"

}
```

```
POST /test/article/2

{

    "title":"大米手机",

    "images":"http://image.leyou.com/12479122.jpg",

    "price":2899.00

}
```
 

修改数据，必须指定id

```
PUT test/article/2

{

  "title": "我爱她",

}
```

删除数据

```
DELETE /索引库名/类型名/id值
```

 

## 数据查询

"took": 1,  查询花费时间，单位是毫秒

"timed_out": false,  是否超时

"_shards": { "total": 5,"successful": 5,"skipped": 0,"failed": 0 },分片信息

"hits": {   搜索结果总览对象

"total": 5,  搜索到的总条数

"max_score": 1,  所有结果中文档得分的最高分

"hits": [   搜索结果的文档对象数组，每个元素是一条搜索到的文档信息

{"_index": "test",  索引库

"_type": "article",  文档类型

"_id": "5",  文档id

"_score": 1,  文档得分

"_source": {   文档的源数据

 

查询类型  match_all（查所有）  match（匹配查找）  term（词条查找）  range（范围查找）  multi_match（多字段匹配查找） fuzz（模糊查找）bool（布尔查找，相当于塞了很多条件过滤）

1.基本查询

```
GET /test/_search

{

    "query":{  代表查询对象

        "查询类型":{  

            "查询条件":"查询条件值"

        }

    }

}
```
 

 

match匹配查询，会把查询条件进行分词，然后进行查询, 多个词条之间默认是or的关系

```
GET /test/_search

{

    "query":{

        "match":{

          "content": "昨夜天涯"

        }

    }

}
```

```
GET /test/_search

{

    "query":{

        "match":{

          "content": {"query": "昨夜天涯","operator": "and"} 分词后改为and关系会比较精确

        }

}

}
```
 

还可以设置匹配参数 minimum_should_match 设置都是百分比，越大越精确

multi_match 多字段匹配查询

```
GET /test/_search

{

    "query":{

        "multi_match":{

          "query": "昨夜天涯",

          "fields": ["title", "content"]

        }

    }

}
```
 

term 词条匹配 一般选不能进行分词的字段进行匹配，即keyword类型

```
GET /test/_search

{

    "query":{

        "term": {

          "price":1  选择的字段最好是数字，时间，布尔值，或者是不能分词的字符串

        }

    }

}
```
 

"terms":相当于sql里面的in

```
GET /test/_search

{

    "query":{

        "terms":{

            "price":[2699.00,2899.00,3899.00]

        }

    }

}
```
 

 

2._source过滤，相当于查询数据里的数据时候，只查询几个字段，其他字段不返回一样

```
GET /test/_search

{

  "_source": ["content"],

  "query":{"match": {"title":"hahaha"}}

}
```

而且source还可以添加includes和excludes

```
GET /heima/_search

{
  "_source": {"includes":["title","price"]},
  "query": {
    "term": {"price": 2699}
  }
}
```
 

 

3.高级查询

布尔组合
```
GET /test/_search
{

    "query":{

        "bool":{

         "must":    { "match": { "title": "大米" }},

         "must_not": { "match": { "title":  "电视" }},

         "should":   { "match": { "title": "手机" }}

        }

    }

}
```
 

range查询 查询找出那些落在指定区间内的数字或者时间

```
GET /test/_search

{

    "query":{

        "range": {"price": {"gte":  1000.0,"lt":   2800.00}}

    }

}
```
 

fuzzy 查询 针对不分词的字段

fuzzy 查询是 term 查询的模糊等价。它允许用户搜索词条与实际词条的拼写出现偏差，但是偏差的编辑距离不得超过2：

```
GET /test/_search

{

  "query": {

    "fuzzy": {"title": "appla"}

  }

}
```

```
GET /heima/_search

{

  "query": {

    "fuzzy": {

        "title": {

            "value":"appla",

            "fuzziness":1   通过fuzziness来指定允许的编辑距离

        }

    }

  }

}
```
 

filter 过滤一定要发生在布尔查询中，可以把过滤条件认为是布尔条件的一个组合条件

```
GET /test/_search

{

    "query":{

        "bool":{

         "must":{ "match": { "title": "小米手机" }},

         "filter":{"range":{"price":{"gt":2000.00,"lt":3800.00}}}

        }

    }

}
```
 

## 排序

sort 可以让我们按照不同的字段进行排序，并且通过order指定排序的方式

```
GET /test/_search

{

  "query": {
    "match": {"title": "小米手机"}
    },

  "sort": [
    {
    "price": {"order": "desc"}
    }
    ]
}
```
 

## 高亮

```
GET /test/_search

{

    "query":{"match": {"title": "手机"}},

    "highlight": {"fields": {"title": {}}}

}
```
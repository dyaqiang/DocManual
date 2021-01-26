#### 1、核心概念解释

##### 1.1、索引 index

对应数据库中

##### 1.2、类型 type

对应数据表

##### 1.3、字段 Field

对应字段

##### 1.4、映射 mapping

对应字段的一些设置、分词、索引、

##### 1.5 文档 Document

##### 1.6、接近实时 index

##### 1.7、集群 cluster

##### 1.8 、节点 node

##### 1.9 、分片 复制

#### 2、 操作数据

##### 2.1 创建索引

```http
put: http://localhoust:9200/myapp01
```

##### 2.2 创建mapping

```json
{
    "mappings":{
        "article":{
            "properties":{
            "id":{
                "type":"long",
                "store":true,
                "index":"not_analyzed"
            },
            "title":{
                "type":"text",
                "store":true,
                "index":"analyzed",
                "analyzer":"standard"
            },
             "content":{
                "type":"text",
                "store":true,
                "index":"analyzed",
                "analyzer":"standard"
            }
            }
       }
  }
}
put: http://localhoust:9200/myapp01
```



















```
* 索引(index)---对应数据库
* 类型(type)---对应表
* 文档(document)---对应行
* fields-- 字段
```








## 第四节 QueryDSL 基本使用




### 4.1 基本语法格式

Elasticsearch 提供了一个可以执行查询的 Json 风格的 DSL。这个被称为 Query DSL，该查询语言非常全面。

一个查询语句的典型结构:

```json
{
   QUERY_NAME:{
      ARGUMENT:VALUE,
      ARGUMENT:VALUE,...
   }
}
```

如果针对于某个字段，那么它的结构如下：

```json
{
  QUERY_NAME:{
     FIELD_NAME:{
       ARGUMENT:VALUE,
       ARGUMENT:VALUE,...
      }   
   }
}
```

#### 举例

```json
GET bank/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "balance": {
        "order": "desc"
      }
    }
  ],
  "from": 0,
  "size": 5,
  "_source": ["balance","firstname"]
}
```

* `query`：查询，`match_all` 查询类型【代表查询所有的所有】，es 中可以在 query 中组合非常多的查询类型完成复杂查询。

* `sort`：排序规则

* `from` 与 `size`：分页相关

* `_source`：指定返回哪些字段


### 4.2 match 全文检索



### 4.3 



### 4.4 
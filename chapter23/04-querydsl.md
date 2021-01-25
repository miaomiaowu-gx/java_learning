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

#### 4.2.1 基本类型（非字符串），精确控制

```json
GET bank/_search
{
  "query": {
    "match": {
      "account_number": 20
    }
  }
}
```

检索出一条数据，其 `account_number` 值为 20。

#### 4.2.2 字符串，全文检索(模糊查询)

```json
GET bank/_search
{
  "query": {
    "match": {
      "address": "Kings"
    }
  }
}
##全文检索最终会按照评分进行排序，会对检索条件进行分词匹配。
```

检索出两条数据，其 `address` 中含有 `Kings`。两条数据的 `address` 字段如下：

```
"address" : "282 Kings Place"
"address" : "305 Kings Hwy"
```

> 注：当检索词多与 1 个时，包含其中**任意一个**单词的数据都会被检索出来。

### 4.3 `match_phrase` 短语匹配

```json
GET bank/_search
{
  "query": {
    "match_phrase": {
      "address": "mill lane"
    }
  }
}
```

* `mill lane` 以短语形式进行匹配。

### 4.4 `multi_match` 多字段匹配



### 4.5 bool 复合查询

### 4.6 filter 过滤

### 4.7 term 查询

### 4.8 aggregations 聚合分析


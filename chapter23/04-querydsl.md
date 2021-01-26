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

* `mill lane` 以短语形式进行匹配。单词不再拆分检索，必须两者同时以短语形式出现，才会被检索到。

#### 文本的精确匹配 `keyword`

文本字段的匹配，使用 keyword，匹配的条件要显示字段的全部值，即进行精确匹配的。

```json
GET bank/_search
{
  "query": {
    "match_phrase": {
      "address.keyword": "198 Mill Lane"
    }
  }
}
```

* 只有 `address` 字段值**等于** `198 Mill Lane` 时，才会被检索到。不再是模糊匹配。


### 4.4 `multi_match` 多字段匹配

```json
GET bank/_search
{
  "query": {
    "multi_match": {
      "query": "mill",
      "fields": [
        "state",
        "address"
      ]
    }
  }
}
```

state 或者 address 中包含 mill，并且在查询过程中，会对于查询条件进行**分词**。

### 4.5 bool 复合查询

复合语句可以合并任何其他查询语句，包括复合语句。即复合语句之间可以互相嵌套，表达非常复杂的逻辑。

1. `must`：必须达到 `must` 所列举的所有条件。

2. `must_not`，必须不匹配 `must_not` 所列举的所有条件。

3. `should`，应该满足 should 所列举的条件。

1）查询 `gender=m`，并且 `address=mill` 的数据

```json
GET bank/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "gender": "M"
          }
        },
        {
          "match": {
            "address": "mill"
          }
        }
      ]
    }
  }
}
```

2）查询 `gender=m`，并且 `address=mill` 的数据，但是 `age!=38`。

```json
GET bank/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "gender": "M"
          }
        },
        {
          "match": {
            "address": "mill"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "age": "38"
          }
        }
      ]
    }
  }
} 
```


3）匹配 `lastName` 应该等于 `Wallace` 的数据。

```json
GET bank/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "gender": "M"
          }
        },
        {
          "match": {
            "address": "mill"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "age": "18"
          }
        }
      ],
      "should": [
        {
          "match": {
            "lastname": "Wallace"
          }
        }
      ]
    }
  }
}
```

`should`：应该达到 should 列举的条件，如果到达会增加相关文档的评分，并不会改变查询的结果。没有匹配到 `should` 的数据也会被返回。



### 4.6 filter 过滤




### 4.7 term 查询




### 4.8 aggregations 聚合分析





## 第三节 Search Api

&emsp;&emsp;进入 Elasticsearch 官网，点击【文档】，在[Elasticsearch: Store, Search, and Analyze]下点击[Elasticsearch Reference [7.10]]。[参考链接](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)


**ES 支持两种基本方式检索**：

* 通过 REST request uri 发送搜索参数（uri +检索参数）；
* 通过 REST request body 来发送它们（uri+请求体）；


### 3.1 一切检索从 `_search`

<img src="./img23/10-search-first.png" width=600>


#### 3.1.1 请求参数方式检索

```
GET bank/_search?q=*&sort=account_number:asc
```

* 在 `bank` 索引下检索，`q=*` 表示查询所有，`sort=account_number:asc` 表示按照 `account_number` 字段升序排列。



#### 3.1.1 



### 3.2 



### 3.3 
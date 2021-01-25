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

### 4.2



### 4.3 



### 4.4 
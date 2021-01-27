## 第八节 测试复杂检索

### 8.1 聚合操作

用 java 代码实现下列检索：

```json
## 按照年龄聚合，并且求这些年龄段的这些人的平均薪资
GET bank/_search
{
  "query": {
    "match_all": {}
  },
  "aggs": {
    "ageAgg": {
      "terms": {
        "field": "age",
        "size": 100
      },
      "aggs": {
        "ageAvg": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  },
  "size": 0
}
```


### 8.2 





### 8.3 

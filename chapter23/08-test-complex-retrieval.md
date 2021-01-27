## 第八节 测试复杂检索

### 8.1 聚合操作

用 java 代码实现下列检索：

```json
## 搜索 `address` 中包含 `mill` 的所有人的年龄分布以及平均年龄
GET bank/_search
{
  "query": {
    "match": {
      "address": "Mill"
    }
  },
  "aggs": {
    "ageAgg": {
      "terms": {
        "field": "age",
        "size": 10
      }
    },
    "ageAvg": {
      "avg": {
        "field": "age"
      }
    },
    "balanceAvg": {
      "avg": {
        "field": "balance"
      }
    }
  },
  "size": 0
}
```


### 8.2 





### 8.3 

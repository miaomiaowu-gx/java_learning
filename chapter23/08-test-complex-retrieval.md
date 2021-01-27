## 第八节 测试复杂检索

### 8.1 聚合操作

#### 8.1.1 检索

用 java 代码实现下列检索：

```json
## 搜索 `address` 中包含 `mill` 的所有人的年龄分布以及平均年龄
GET bank/_search
{
  "query": {
    "match": {
      "address": "mill"
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
  }
}
```

#### 8.1.2 检索结果

```json
{
  "took" : 6,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 5.4032025,
    "hits" : [
      {
        "_index" : "bank",
        "_type" : "account",
        "_id" : "970",
        "_score" : 5.4032025,
        "_source" : {
          "account_number" : 970,
          "balance" : 19648,
          "firstname" : "Forbes",
          "lastname" : "Wallace",
          "age" : 28,
          "gender" : "M",
          "address" : "990 Mill Road",
          "employer" : "Pheast",
          "email" : "forbeswallace@pheast.com",
          "city" : "Lopezo",
          "state" : "AK"
        }
      },
     ...
    ]
  },
  "aggregations" : {
    "ageAgg" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : 38,
          "doc_count" : 2
        },
        {
          "key" : 28,
          "doc_count" : 1
        },
        {
          "key" : 32,
          "doc_count" : 1
        }
      ]
    },
    "ageAvg" : {
      "value" : 34.0
    },
    "balanceAvg" : {
      "value" : 25208.0
    }
  }
}
```


### 8.2 java 实现

[Search API](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-search.html)

```java
package com.atguigu.gulimall.search;

import ...

@RunWith(SpringRunner.class)
@SpringBootTest
public class GulimallSearchApplicationTests {

	@Autowired
	private RestHighLevelClient client;

	@Test
	public void searchData() throws IOException {
		//1、创建检索请求
		SearchRequest searchRequest = new SearchRequest();
		//指定索引
		searchRequest.indices("bank");
		//指定DSL，检索条件 SearchSourceBuilder
		SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
		//1.1 构造检索条件
		sourceBuilder.query(QueryBuilders.matchQuery("address","mill"));
		//1.2 按照年龄的值分布进行聚合
		TermsAggregationBuilder ageAgg = AggregationBuilders.terms("ageAgg").field("age").size(10);
		sourceBuilder.aggregation(ageAgg);
		//1.3 计算平均年龄
		AvgAggregationBuilder ageAvg = AggregationBuilders.avg("ageAvg").field("age");
		sourceBuilder.aggregation(ageAvg);
		//1.4 计算平均薪资
		AvgAggregationBuilder balanceAvg = AggregationBuilders.avg("balanceAvg").field("balance");
		sourceBuilder.aggregation(balanceAvg);

		System.out.println("检索条件："+sourceBuilder.toString());
		searchRequest.source(sourceBuilder);

		//2、执行检索
		SearchResponse searchResponse = client.search(searchRequest, GulimallElasticSearchConfig.COMMON_OPTIONS);

		//3、分析结果 searchResponse
		System.out.println("响应结果："+searchResponse.toString());
		//3.1 获取所有查到的数据
		SearchHits hits = searchResponse.getHits();
		SearchHit[] searchHits = hits.getHits();
		for (SearchHit hit : searchHits) {
			/**
			 * "_index" : "bank",
			 * "_type" : "account",
			 * "_id" : "970",
			 * "_score" : 5.4032025,
			 * "_source" : {}
			 */
			//hit.getIndex();hit.getType();hit.getId();hit.getScore();
			String jsonString = hit.getSourceAsString(); //返回 JSON 串
			Account account = JSON.parseObject(jsonString, Account.class);
			System.out.println("account："+account);
		}
		//3.2 获取本次检索的分析信息（聚合信息）
		Aggregations aggregations = searchResponse.getAggregations();
//		for (Aggregation aggregation : aggregations.asList()) {
//			System.out.println("当前聚合名字："+aggregation);
//		}
		Terms ageAgg1 = aggregations.get("ageAgg");
		for (Terms.Bucket bucket : ageAgg1.getBuckets()) {
			String keyAsString = bucket.getKeyAsString();
			System.out.println("年龄："+keyAsString+"==>"+bucket.getDocCount());
		}
		Avg balanceAvg1 = aggregations.get("balanceAvg");
		System.out.println("平均薪资："+balanceAvg1.getValue());
	}

	@ToString
	@Data
	static class Account {
		private int account_number;
		private int balance;
		private String firstname;
		private String lastname;
		private int age;
		private String gender;
		private String address;
		private String employer;
		private String email;
		private String city;
		private String state;
	}

}
```






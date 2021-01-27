## 第七节 SpringBoot 整合 high-level-client

### 7.1 Elasticsearch-Rest-Client

#### 7.1.1 9300: TCP(不建议使用)

`spring-data-elasticsearch:transport-api.jar;`

不推荐使用，原因如下：

* springboot 版本不同，ransport-api.jar 不同，不能适配 es 版本。

* 7.x 已经不建议使用，8 以后就要废弃。

#### 7.1.2 9200: HTTP

* `jestClient`: 非官方，更新慢；

* `RestTemplate`：模拟 HTTP 请求，ES 很多操作需要自己封装，麻烦；

* `HttpClient`：同上；

* `Elasticsearch-Rest-Client`：官方 RestClient，封装了 ES 操作，API 层次分明，上手简单；

最终选择 `Elasticsearch-Rest-Client`（elasticsearch-rest-high-level-client）； 

https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high.html

### 7.2 创建 `gulimall-search` 模块

#### 7.2.1 导入依赖

<img src="./img23/15-gulimall-search-module.png" >

1）在 `gulimall` 中添加 `gulimall-search` 模块

```xml
<module>gulimall-search</module>
```

2）修改 `spring-boot-starter-parent` 版本，与 `gulimall` 项目统一。

```xml
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.1.8.RELEASE</version>
	<relativePath/> <!-- lookup parent from repository -->
</parent>
```

3）在 `gulimall-search` 模块的 `pom` 中引入 `elasticsearch` 坐标

```xml
<dependency>
	<groupId>org.elasticsearch.client</groupId>
	<artifactId>elasticsearch-rest-high-level-client</artifactId>
	<version>7.6.2</version>
</dependency>
```

4）在 `gulimall-search` 模块的 `pom` 中添加 `elasticsearch` 约束。

```xml
<properties>
	<elasticsearch.version>7.6.2</elasticsearch.version>
</properties>
```

<img src="./img23/16-elasticsearch-sb-version.png" width=600>


#### 7.2.2 配置

`gulimall-search` 中：

1）导入 `gulimall-common` 坐标。

2）在 `resources` 下 `application.properties` 中添加注册中心：

```properties
spring.cloud.nacos.discovery.server-addr=127.0.0.1:8848
spring.application.name=gulimall-search
```


3）在 `com.atguigu.gulimall.search` 下，创建包 `config`，创建配置类 `GulimallElasticSearchConfig`：

```java
package com.atguigu.gulimall.search.config;


import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestClientBuilder;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 1. 导入依赖
 * 2. 编写配置，给容器中注入一个RestHighLevelClient
 * 3. 参照官方 API
 */

@Configuration
public class GulimallElasticSearchConfig {

    @Bean
    public RestHighLevelClient esRestClient(){
        RestClientBuilder builder = RestClient.builder(new HttpHost("192.168.56.10", 9200, "http"));
        RestHighLevelClient client = new RestHighLevelClient(builder);
        return client;
    }
}
```

4）启动类

```java
package com.atguigu.gulimall.search;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@EnableDiscoveryClient
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
public class GulimallSearchApplication {
	public static void main(String[] args) {
		SpringApplication.run(GulimallSearchApplication.class, args);
	}
}
```


5）测试类测试

```java
package com.atguigu.gulimall.search;


import org.elasticsearch.client.RestHighLevelClient;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class GulimallSearchApplicationTests {

	@Autowired
	private RestHighLevelClient client;
	@Test
	public void contextLoads() {
		System.out.println(client);
	}

}
```

输出：

```
org.elasticsearch.client.RestHighLevelClient@75babb67
```



具体操作参考：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high.html



### 7.3 测试保存

在配置类中添加 `RequestOptions`：

```java
package com.atguigu.gulimall.search.config;

import org.apache.http.HttpHost;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestClientBuilder;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 1. 导入依赖
 * 2. 编写配置，给容器中注入一个RestHighLevelClient
 * 3. 参照官方 API
 */

@Configuration
public class GulimallElasticSearchConfig {

    //a singleton instance and share it between all requests

    public static final RequestOptions COMMON_OPTIONS;
    static {
        RequestOptions.Builder builder = RequestOptions.DEFAULT.toBuilder();
//        builder.addHeader("Authorization", "Bearer " + TOKEN);
//        builder.setHttpAsyncResponseConsumerFactory(
//                new HttpAsyncResponseConsumerFactory
//                        .HeapBufferedResponseConsumerFactory(30 * 1024 * 1024 * 1024));
        COMMON_OPTIONS = builder.build();
    }

    @Bean
    public RestHighLevelClient esRestClient(){
        RestClientBuilder builder = RestClient.builder(new HttpHost("192.168.56.10", 9200, "http"));
        RestHighLevelClient client = new RestHighLevelClient(builder);
        return client;
    }
}
```

#### 7.3.1 在 `kibana` 中查询

```json
GET users/_search
```

返回结果，说明 `users` 不存在

```json
{
  "error" : {
    "root_cause" : [
      {
        "type" : "index_not_found_exception",
        "reason" : "no such index [users]",
        "resource.type" : "index_or_alias",
        "resource.id" : "users",
        "index_uuid" : "_na_",
        "index" : "users"
      }
    ],
    "type" : "index_not_found_exception",
    "reason" : "no such index [users]",
    "resource.type" : "index_or_alias",
    "resource.id" : "users",
    "index_uuid" : "_na_",
    "index" : "users"
  },
  "status" : 404
}
```

#### 7.3.2 保存操作

```java
package com.atguigu.gulimall.search;

import com.alibaba.fastjson.JSON;
import com.atguigu.gulimall.search.config.GulimallElasticSearchConfig;
import lombok.Data;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.common.xcontent.XContentType;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.io.IOException;

@RunWith(SpringRunner.class)
@SpringBootTest
public class GulimallSearchApplicationTests {

	@Autowired
	private RestHighLevelClient client;

	/**
	 * 测试[存储]数据到 es
	 * 再次发送时，是[更新]操作
	 */
	@Test
	public void indexData() throws IOException {
		IndexRequest indexRequest = new IndexRequest("users"); //根据索引建立请求
		indexRequest.id("1"); //数据的id
		//方式一：直接使用k-v对
		//indexRequest.source("userName","gx","age",18,"gender","女");
		//方式二：
		User user = new User();
		user.setUserName("白敬亭");
		user.setGender("男");
		user.setAge(28);
		String jsonString = JSON.toJSONString(user);
		indexRequest.source(jsonString, XContentType.JSON);
		//执行保存操作(同步)
		IndexResponse index = client.index(indexRequest, GulimallElasticSearchConfig.COMMON_OPTIONS);
		//提取有用的响应数据
		System.out.println(index);
	}

	@Data
	class User{
		private String userName;
		private String gender;
		private Integer age;
	}
}
```

再次查询 `GET users/_search` 时：

```json
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "users",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "age" : 28,
          "gender" : "男",
          "userName" : "白敬亭"
        }
      }
    ]
  }
}
```  
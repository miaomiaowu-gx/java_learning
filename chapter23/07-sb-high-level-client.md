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

### 7.2 






### 7.3 
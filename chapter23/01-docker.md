## 第一节 Docker安装 ES

增大虚拟机内存至 1G。

### 3.1 Elasticsearch


1）`vagrant ssh` 链接虚拟机。


2）下载 ealastic search 和 kibana

```shell
sudo docker pull elasticsearch:7.6.2
```

3）检查虚拟机内存 `free -m`

```shell
available
376
```

4）配置

```shell
mkdir -p /mydata/elasticsearch/config
mkdir -p /mydata/elasticsearch/data
echo "http.host: 0.0.0.0" >/mydata/elasticsearch/config/elasticsearch.yml
chmod -R 777 /mydata/elasticsearch/
```


5）启动 elasticsearch

```shell
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \
-e  "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms64m -Xmx512m" \
-v /mydata/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /mydata/elasticsearch/data:/usr/share/elasticsearch/data \
-v  /mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.6.2 
```


6）设置开机启动 elasticsearch

```shell
docker update elasticsearch --restart=always
```

7）浏览器访问 `http://192.168.56.10:9200`

```json
{
  "name" : "d248687cf2f9",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "XXM1pkC1QVKuBDdtRcj7pQ",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

8）Postman 发送请求 `http://192.168.56.10:9200/_cat/nodes`。

```
127.0.0.1 15 90 1 0.00 0.01 0.05 dilm * d248687cf2f9
```

### 3.2 Kibana

&emsp;&emsp;Kibana 是一个免费且开放的用户界面，能够对 Elasticsearch 数据进行可视化，并在 Elastic Stack 中进行导航。可以进行各种操作，从跟踪查询负载，到理解请求如何流经整个应用，都能轻松完成。

1）下载 kibana

```shell
sudo docker pull kibana:7.6.2
```


2）启动 kibana

```shell
docker run --name kibana -e ELASTICSEARCH_HOSTS=http://192.168.56.10:9200 -p 5601:5601 -d kibana:7.6.2
```

* ELASTICSEARCH_HOSTS 设置为自己的虚拟机地址

3）设置开机启动 kibana

```shell
docker update kibana  --restart=always
```

4）浏览器访问 `http://192.168.56.10:5601/app/kibana`




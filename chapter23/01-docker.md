## 第一节 Docker安装 ES

增大虚拟机内存至 1G。

### 3.1 Elasticsearch


1）`vagrant ssh` 链接虚拟机。


2）下载 ealastic search 和 kibana

```shell
sudo docker pull elasticsearch:7.6.2
sudo docker pull kibana:7.6.2
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

### 3.2 Kibana

1）


2）


3）


4）


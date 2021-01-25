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


5）安装 Elasticsearch









6）


7）


8）


9）
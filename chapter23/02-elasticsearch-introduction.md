## 第二节 ElasticSearch 入门


### 2.1 `_cat`


1）`/_cat/nodes`：查看所有节点

```
GET
http://192.168.56.10:9200/_cat/nodes

结果
127.0.0.1 15 94 1 0.07 0.03 0.07 dilm * d248687cf2f9
```



2）`/_cat/health`：查看健康状态

```
GET  
http://192.168.56.10:9200/_cat/health

结果
1611560267 07:37:47 elasticsearch green 1 1 3 3 0 0 0 0 - 100.0%
```



3）`/_cat/master`：查看主节点信息

```
GET 
http://192.168.56.10:9200/_cat/master

结果
XDjdbKv-QZKoZDKKNCebqQ 127.0.0.1 127.0.0.1 d248687cf2f9
```


4）`/_cat/indices`：查看索引

```
GET 
http://192.168.56.10:9200/_cat/indices

结果
green open .kibana_task_manager_1   DPlST-F7Q0iwj-px4ryAUw 1 0 2 1 37.9kb 37.9kb
green open .apm-agent-configuration uHYGmwXzSA6qmfYFVkUBZw 1 0 0 0   283b   283b
green open .kibana_1                CSQjGYkfTP2GwuAgfk0-PA 1 0 7 0 31.2kb 31.2kb
```

### 2.2 put & post新增数据

索引一个文档（保存）：保存一个数据，保存在哪个索引的哪个类型下，指定用哪个唯一标识。如 `PUT customer/external/1;` 在 customer 索引下的 external 类型下保存 1 号数据为

```json
PUT customer/external/1

请求参数
{
 "name":"John Doe"
}
```


### 2.3 get 查询数据 & 乐观锁字段

### 2.4 put & post 修改数据

### 2.5 删除数据 & bulk 批量操作导入样本测试数据


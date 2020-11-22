## 第一节 命令操作

### 1.1 Redis 数据结构

redis 存储的是：key,value 格式的数据，其中 key 都是字符串，value 有 5 种不同的数据结构：

1) 字符串类型 string
2) 哈希类型 hash ：map 格式  
3) 列表类型 list ：linkedlist 格式。支持重复元素
4) 集合类型 set ：不允许重复元素
5) 有序集合类型 sortedset：不允许重复元素，且元素有顺序

#### 1.1.1 字符串类型 string

1. 存储：`set key value`

```shell
127.0.0.1:6379> set username zhangsan
OK
```

2. 获取：`get key`

```shell
127.0.0.1:6379> get username
"zhangsan"
```
			
3. 删除：`del key`

```shell
127.0.0.1:6379> del age
(integer) 1
127.0.0.1:6379> get username
(nil)
```


#### 1.1.2 哈希类型 hash







#### 1.1.3


#### 1.1.4


#### 1.1.5    
  
    
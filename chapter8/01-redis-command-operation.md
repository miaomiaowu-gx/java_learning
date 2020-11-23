## 第一节 命令操作

### 1.1 Redis 数据结构

redis 存储的是：key,value 格式的数据，其中 key 都是字符串，value 有 5 种不同的数据结构：

1) 字符串类型 string
2) 哈希类型 hash ：map 格式  
3) 列表类型 list ：linkedlist 格式。支持重复元素
4) 集合类型 set ：不允许重复元素
5) 有序集合类型 sortedset：不允许重复元素，且元素有顺序

#### 1.1.1 字符串类型 string

**1. 存储**：`set key value`

```bash
127.0.0.1:6379> set username zhangsan
OK
```

**2. 获取**：`get key`

```shell
127.0.0.1:6379> get username
"zhangsan"
```
			
**3. 删除**：`del key`

```shell
127.0.0.1:6379> del username
(integer) 1
127.0.0.1:6379> get username
(nil)
```


#### 1.1.2 哈希类型 hash


**1. 存储**：`hset key field value`

```shell
127.0.0.1:6379> hset myhash username lisi
(integer) 1
127.0.0.1:6379> hset myhash password 123
(integer) 1
```

**2. 获取**： 

* `hget key field`: 获取指定的 field 对应的值

```shell
127.0.0.1:6379> hget myhash username
"lisi"
```

* `hgetall key`：获取所有的 field 和 value

```shell
127.0.0.1:6379> hgetall myhash
1) "username"
2) "lisi"
3) "password"
4) "123"
```				
**3. 删除**：`hdel key field`

```shell
127.0.0.1:6379> hdel myhash username
(integer) 1
```



#### 1.1.3 列表类型 list

可以添加一个元素到列表的头部（左边）或者尾部（右边）

**1. 添加**：

* `lpush key value`: 将元素加入列表左表
	
* `rpush key value`：将元素加入列表右边

```shell	
127.0.0.1:6379> lpush myList a
(integer) 1
127.0.0.1:6379> lpush myList b
(integer) 2
127.0.0.1:6379> rpush myList c
(integer) 3
```

**2. 获取**：

* `lrange key start end` ：范围获取

```shell
127.0.0.1:6379> lrange myList 0 -1
1) "b"
2) "a"
3) "c"
```

**3. 删除**：

* `lpop key`：删除列表最左边的元素，并将元素返回
* `rpop key`：删除列表最右边的元素，并将元素返回



#### 1.1.4 集合类型 set ：不允许重复元素

**1. 存储**：`sadd key value`

```shell
127.0.0.1:6379> sadd myset a
(integer) 1
127.0.0.1:6379> sadd myset a
(integer) 0
```

**2. 获取**：`smembers key`，获取 set 集合中所有元素

```shell
127.0.0.1:6379> smembers myset
1) "a"
```

**3. 删除**：`srem key value`，删除 set 集合中的某个元素	

```shell
127.0.0.1:6379> srem myset a
(integer) 1
```



#### 1.1.5 有序集合类型 sortedset   

不允许重复元素，且元素有顺序。每个元素都会关联一个 double 类型的分数。redis 正是通过分数来为集合中的成员进行从小到大的排序。

**1. 存储**：`zadd key score value`

```shell
127.0.0.1:6379> zadd mysort 60 zhangsan
(integer) 1
127.0.0.1:6379> zadd mysort 50 lisi
(integer) 1
127.0.0.1:6379> zadd mysort 80 wangwu
(integer) 1
```

**2. 获取**：`zrange key start end [withscores]`

```shell
127.0.0.1:6379> zrange mysort 0 -1
1) "lisi"
2) "zhangsan"
3) "wangwu"

127.0.0.1:6379> zrange mysort 0 -1 withscores
1) "zhangsan"
2) "60"
3) "wangwu"
4) "80"
5) "lisi"
6) "500"
```

**3. 删除**：`zrem key value`

```shell
127.0.0.1:6379> zrem mysort lisi
(integer) 1
```

#### 1.1.6 通用命令

1. `keys *` : 查询所有的键

2. `type key` ： 获取键对应的 value 的类型

3. `del key` ：删除指定的 key value 


### 1.2 持久化

&emsp;&emsp;Redis 是一个内存数据库，当 Redis 服务器重启，获取电脑重启，数据会丢失，可以将 Redis 内存中的数据持久化保存到硬盘的文件中。


#### 1.2.1 redis 持久化机制

🍒 RDB：默认方式，不需要进行配置，默认就使用这种机制。在一定的间隔时间中，检测 key 的变化情况，然后持久化数据。

1. 编辑 redis.windwos.conf 文件

```conf
#   after 900 sec (15 min) if at least 1 key changed
save 900 1
#   after 300 sec (5 min) if at least 10 keys changed
save 300 10
#   after 60 sec if at least 10000 keys changed
save 60 10000
```
				
2. 重新启动 redis 服务器，并指定配置文件名称

```
D:\JavaWeb2018\day23_redis\资料\redis\windows-64\redis-2.8.9>redis-server.exe redis.windows.conf	
```			

🍒 AOF：日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据。

1. 编辑 redis.windwos.conf 文件

```
appendonly no（关闭aof） --> appendonly yes （开启aof）
# appendfsync always ： 每一次操作都进行持久化
appendfsync everysec ： 每隔一秒进行一次持久化
# appendfsync no	 ： 不进行持久化
```
  
    
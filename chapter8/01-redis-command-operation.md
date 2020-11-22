## 第一节 命令操作

### 1.1 Redis 数据结构

redis 存储的是：key,value 格式的数据，其中 key 都是字符串，value 有 5 种不同的数据结构：

1) 字符串类型 string
2) 哈希类型 hash ：map 格式  
3) 列表类型 list ：linkedlist 格式。支持重复元素
4) 集合类型 set ：不允许重复元素
5) 有序集合类型 sortedset：不允许重复元素，且元素有顺序
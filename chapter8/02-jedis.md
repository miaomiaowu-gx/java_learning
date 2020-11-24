## 第二节 Jedis

Jedis: 一款 java 操作 redis 数据库的工具。

使用步骤

1) 下载 jedis 的 jar 包

* jedis-2.7.0.jar

* commons-pool2-2.3.jar

2) 使用

//1. 获取连接
Jedis jedis = new Jedis("localhost",6379);
//2. 操作
jedis.set("username","zhangsan");
//3. 关闭连接
jedis.close();
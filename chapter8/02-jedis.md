## 第二节 Jedis

Jedis: 一款 java 操作 redis 数据库的工具。

使用步骤

1) 下载 jedis 的 jar 包

* jedis-2.7.0.jar

* commons-pool2-2.3.jar

2) 使用

```java
package cn.itcast.jedis.test;

import redis.clients.jedis.Jedis;

/**
 * jedis的测试类
 */
public class JedisTest {
    // 快速入门
    @Test
    public void test1(){
        //1. 获取连接
        Jedis jedis = new Jedis("localhost",6379);
        //2. 操作
        jedis.set("username","zhangsan");
        //3. 关闭连接
        jedis.close();
    }    
}    
```    
    
### 2.1 Jedis 操作    

#### 2.1.1 操作 string  

#### 2.1.2 操作 hash  

#### 2.1.3 操作 list

#### 2.1.4 操作 set

#### 2.1.5 操作 sortedset

  



### 2.2 Jedis 连接池
    
    
### 2.3 案例    
    
    
    
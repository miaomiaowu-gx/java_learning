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
import org.junit.Test;

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
    
### 2.1 Jedis 操作各种 Redis 中的数据结构    

#### 2.1.1 操作 string  

```java
/**
 * string 数据结构操作
 */
@Test
public void test2(){
    //1. 获取连接
    Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379 端口
    //2. 操作
    //存储
    jedis.set("username","zhangsan");
    //获取
    String username = jedis.get("username");
    System.out.println(username);

    //可以使用setex()方法存储可以指定过期时间的 key value
    jedis.setex("activecode",20,"hehe");//将activecode：hehe键值对存入redis，并且20秒后自动删除该键值对

    //3. 关闭连接
    jedis.close();
}
```

#### 2.1.2 操作 hash  

```java
/**
 * hash 数据结构操作
 */
@Test
public void test3(){
    //1. 获取连接
    Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
    //2. 操作
    // 存储hash
    jedis.hset("user","name","lisi");
    jedis.hset("user","age","23");
    jedis.hset("user","gender","female");

    // 获取hash
    String name = jedis.hget("user", "name");
    System.out.println(name);
    
    // 获取hash的所有map中的数据
    Map<String, String> user = jedis.hgetAll("user");

    // keyset
    Set<String> keySet = user.keySet();
    for (String key : keySet) {
        //获取value
        String value = user.get(key);
        System.out.println(key + ":" + value);
    }

    //3. 关闭连接
    jedis.close();
}
```


#### 2.1.3 操作 list

```java
/**
 * list 数据结构操作
 */
@Test
public void test4(){
    //1. 获取连接
    Jedis jedis = new Jedis();
    //2. 操作
    // list 存储
    jedis.lpush("mylist","a","b","c");//从左边存
    jedis.rpush("mylist","a","b","c");//从右边存

    // list 范围获取
    List<String> mylist = jedis.lrange("mylist", 0, -1);
    System.out.println(mylist);
    
    // list 弹出
    String element1 = jedis.lpop("mylist");//c
    System.out.println(element1);

    String element2 = jedis.rpop("mylist");//c
    System.out.println(element2);

    // list 范围获取
    List<String> mylist2 = jedis.lrange("mylist", 0, -1);
    System.out.println(mylist2);

    //3. 关闭连接
    jedis.close();
}
```


#### 2.1.4 操作 set

```java
/**
 * set 数据结构操作
 */
@Test
public void test5(){
    //1. 获取连接
    Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
    
    //2. 操作
    // set 存储(可以一次存多个)
    jedis.sadd("myset","java","php","c++");

    // set 获取
    Set<String> myset = jedis.smembers("myset");
    System.out.println(myset);

    //3. 关闭连接
    jedis.close();
}
```

#### 2.1.5 操作 sortedset

```java
/**
 * sortedset 数据结构操作
 */
@Test
public void test6(){
    //1. 获取连接
    Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
    
    //2. 操作
    // sortedset 存储
    jedis.zadd("mysortedset",3,"亚瑟");
    jedis.zadd("mysortedset",30,"后羿");
    jedis.zadd("mysortedset",55,"孙悟空");

    // sortedset 获取
    Set<String> mysortedset = jedis.zrange("mysortedset", 0, -1);
    System.out.println(mysortedset);

    //3. 关闭连接
    jedis.close();
}
```  


### 2.2 Jedis 连接池
    
    
### 2.3 案例    
    
    
    
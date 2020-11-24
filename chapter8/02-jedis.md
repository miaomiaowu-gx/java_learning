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

#### 2.2.1 Jedis 连接池 JedisPool 

```java
/**
 * jedis连接池使用
 */
@Test
public void test7(){

    //0.创建一个配置对象
    JedisPoolConfig config = new JedisPoolConfig();
    config.setMaxTotal(50);
    config.setMaxIdle(10);

    //1.创建Jedis连接池对象
    JedisPool jedisPool = new JedisPool(config,"localhost",6379);

    //2.获取连接
    Jedis jedis = jedisPool.getResource();
    //3. 使用
    jedis.set("hehe","heihei");

    //4. 关闭 归还到连接池中
    jedis.close();;
}
```

jedis 详细配置

```properties
#最大活动对象数     
redis.pool.maxTotal=1000    
#最大能够保持idel状态的对象数      
redis.pool.maxIdle=100  
#最小能够保持idel状态的对象数   
redis.pool.minIdle=50    
#当池内没有返回对象时，最大等待时间    
redis.pool.maxWaitMillis=10000    
#当调用borrow Object方法时，是否进行有效性检查    
redis.pool.testOnBorrow=true    
#当调用return Object方法时，是否进行有效性检查    
redis.pool.testOnReturn=true  
#“空闲链接”检测线程，检测的周期，毫秒数。如果为负值，表示不运行“检测线程”。默认为-1.  
redis.pool.timeBetweenEvictionRunsMillis=30000  
#向调用者输出“链接”对象时，是否检测它的空闲超时；  
redis.pool.testWhileIdle=true  
# 对于“空闲链接”检测线程而言，每次检测的链接资源的个数。默认为3.  
redis.pool.numTestsPerEvictionRun=50  
#redis服务器的IP    
redis.ip=xxxxxx  
#redis服务器的Port    
redis1.port=6379   
```


#### 2.2.2 连接池工具类 


【配置文件】

在 src 下创建 jedis.properties 文件

```properties
host=127.0.0.1
port=6379
maxTotal=50
maxIdle=10
```

【工具类】

```java
package cn.itcast.jedis.util;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

/**
 JedisPool工具类
    加载配置文件，配置连接池的参数
    提供获取连接的方法
 */
public class JedisPoolUtils {

    private static JedisPool jedisPool;

    static{
        //读取配置文件
        InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
        //创建Properties对象
        Properties pro = new Properties();
        //关联文件
        try {
            pro.load(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
        //获取数据，设置到JedisPoolConfig中
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
        //初始化JedisPool
        jedisPool = new JedisPool(config,pro.getProperty("host"),Integer.parseInt(pro.getProperty("port")));
    }
    
    /**
     * 获取连接方法
     */
    public static Jedis getJedis(){
        return jedisPool.getResource();
    }
}
```

【测试代码】

```java
/**
 * jedis连接池工具类使用
 */
@Test
public void test8(){
    //1. 通过连接池工具类获取
    Jedis jedis = JedisPoolUtils.getJedis();

    //2. 使用
    jedis.set("hello","world");

    //3. 关闭 归还到连接池中
    jedis.close();;
}
```


### 2.3 案例 

案例需求

1. 提供 index.html 页面，页面中有一个省份下拉列表。

2. 当页面加载完成后，发送 ajax 请求，加载所有省份。


* 注意：使用 redis 缓存一些不经常发生变化的数据。
		* 数据库的数据一旦发生改变，则需要更新缓存。
			* 数据库的表执行 增删改的相关操作，需要将redis缓存数据情况，再次存入
			* 在service对应的增删改方法中，将redis数据删除。   
      
         
#### 2.3.1 搭建数据库

```sql
CREATE DATABASE day23; -- 创建数据库
USE day23; 			   -- 使用数据库
CREATE TABLE province(   -- 创建表
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20) NOT NULL
	
);
-- 插入数据
INSERT INTO province VALUES(NULL,'北京');
INSERT INTO province VALUES(NULL,'上海');
INSERT INTO province VALUES(NULL,'广州');
INSERT INTO province VALUES(NULL,'陕西');
```


#### 2.3.2 环境准备                       

1 导入相关 jar 包 (web->WEB-INF->lib)

2 导入 JQuery

3 在 src 下创建 druid.properties 配置文件                       
                                           
```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql:///day23
username=root
password=mysql
initialSize=5
maxActive=10
maxWait=3000
```

4 在 src 下创建 jedis.properties 配置文件                                                                                                           
```properties                                             
host=127.0.0.1
port=6379
maxTotal=50
maxIdle=10
```

5 在 src->cn->itcast->util 下导入 JDBCUtils.java 类

```java
package cn.itcast.util;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

/**
 * JDBC工具类 使用Durid连接池
 */
public class JDBCUtils {

    private static DataSource ds ;

    static {
        try {
            //1.加载配置文件
            Properties pro = new Properties();
            //使用ClassLoader加载配置文件，获取字节输入流
            InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
            pro.load(is);

            //2.初始化连接池对象
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接池对象
     */
    public static DataSource getDataSource(){
        return ds;
    }

    /**
     * 获取连接Connection对象
     */
    public static Connection getConnection() throws SQLException {
        return  ds.getConnection();
    }
}
```


#### 2.3.3 文件结构

```markdown
|___ src
| |___ cn.itcast
|   |___ util (JDBCUtils.java)
|   |___ dao (ProvinceDao.java)
|   | |___ impl (ProvinceDaoImpl.java) 
|   |___ service (ProvinceService.java)
|   | |___ impl (ProvinceServiceImpl.java)
|   |
|   |
|   |
|   |
|   |
|   |
|   |
|   |
|   |                                                    
                                                                                          
```

#### 2.3.4 实体类

```java
package cn.itcast.domain;
public class Province {
    private int id;
    private String name;
    public int getId() {return id;}
    public void setId(int id) {this.id = id;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
}
```

#### 2.3.5 持久层

【接口】

```java
package cn.itcast.dao;

import cn.itcast.domain.Province;
import java.util.List;

public interface ProvinceDao {
    public List<Province> findAll();
}
```                                                                                                                                             【接口实现类】   

```java
package cn.itcast.dao.impl;

import cn.itcast.dao.ProvinceDao;
import cn.itcast.domain.Province;
import cn.itcast.util.JDBCUtils;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;

import java.util.List;

public class ProvinceDaoImpl implements ProvinceDao {

    //1.声明成员变量 jdbctemplement
    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());

    @Override
    public List<Province> findAll() {
        //1.定义sql
        String sql = "select * from province ";
        //2.执行sql
        List<Province> list = template.query(sql, new BeanPropertyRowMapper<Province>(Province.class));
        return list;
    }
}
```

#### 2.3.6 Service 层

【接口】

```java
package cn.itcast.service;

import cn.itcast.domain.Province;
import java.util.List;

public interface ProvinceService {
    public List<Province> findAll();
    public String findAllJson();
}
```

【接口实现类】





#### 2.3.7
#### 2.3.8
#### 2.3.9
#### 2.3.10
#### 2.3.11                                                    

  
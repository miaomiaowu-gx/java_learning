## 第四节 

### 3.1 完善 account 案例

#### 3.1.1 案例中添加转账方法

基于 xml 案例代码，见本章第二节。

<img src="./img2/08-transfer.png" >


#### 3.1.2 分析事务的问题

<img src="./img2/09-transactionc-ontrol.png" >

#### 3.1.3 编写连接的工具类 ConnectionUtils

之前的例子，事务控制是在持久层，而实际上事务控制应该在业务层！

在 src->main->java->com->itheima 下创建 `utils.ConnectionUtils` 类，该类是一个连接的工具类，它用于从数据源中获取一个连接，并且实现和线程的绑定。

```java

/**
 * 连接的工具类，它用于从数据源中获取一个连接，并且实现和线程的绑定
 */
public class ConnectionUtils {

    private ThreadLocal<Connection> tl = new ThreadLocal<Connection>();

    private DataSource dataSource;

    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    /**
     * 获取当前线程上的连接
     * @return
     */
    public Connection getThreadConnection() {
        try{
            //1.先从ThreadLocal上获取
            Connection conn = tl.get();
            //2.判断当前线程上是否有连接
            if (conn == null) {
                //3.从数据源中获取一个连接，并且存入ThreadLocal中
                conn = dataSource.getConnection();
                tl.set(conn);
            }
            //4.返回当前线程上的连接
            return conn;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    /**
     * 把连接和线程解绑
     */
    public void removeConnection(){
        tl.remove();
    }
}
```  

#### 3.1.4 编写事务管理工具类

在 src->main->java->com->itheima->utils 下创建 TransactionManager 类，是事务管理相关的工具类，它包含了：开启事务、提交事务、回滚事务和释放连接。

```java
package com.itheima.utils;

/**
 * 和事务管理相关的工具类，它包含了，开启事务，提交事务，回滚事务和释放连接
 */
public class TransactionManager {

    private ConnectionUtils connectionUtils;
    public void setConnectionUtils(ConnectionUtils connectionUtils) {
        this.connectionUtils = connectionUtils;
    }

    /**
     * 开启事务
     */
    public  void beginTransaction(){
        try {
            connectionUtils.getThreadConnection().setAutoCommit(false);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    /**
     * 提交事务
     */
    public  void commit(){
        try {
            connectionUtils.getThreadConnection().commit();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    /**
     * 回滚事务
     */
    public  void rollback(){
        try {
            connectionUtils.getThreadConnection().rollback();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    /**
     * 释放连接
     */
    public  void release(){
        try {
            connectionUtils.getThreadConnection().close();//还回连接池中
            connectionUtils.removeConnection();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

 
#### 3.1.5 编写业务层和持久层事务控制代码并配置 Spring 的 ioc 

   
#### 3.1.6 测试转账并分析案例中的问题    



### 3.2 动态代理




## 第八节 Spring 中的事务控制

Spring 事务控制要明确

* 第一：JavaEE 体系进行分层开发，**事务处理位于业务层**， Spring 提供了分层设计业务层的事务处理解决方案。
* 第二：Spring 框架提供了一组事务控制的接口。这组接口是在 `spring-tx-5.0.2.RELEASE.jar` 中。
* 第三：Spring 的事务控制都是基于 AOP 的，它既可以使用编程的方式实现，也可以使用配置的方式实现。 学习的**重点是使用配置**的方式实现。

### 8.1 Spring 中事务控制的 API 介绍

#### 8.1.1 PlatformTransactionManager

此接口是 Spring 的事务管理器，它里面提供了常用的操作事务的方法：

*  获取事务状态信息：`TransactionStatus getTransaction(TransactionDefinition definition)` 
* 提交事务：`void commit(TransactionStatus status)`	
* 回滚事务：`void rollback(TransactionStatus status)`

在开发中都是使用它的实现类 ，**真正管理事务的对象**：

* `org.springframework.jdbc.datasource.DataSourceTransactionManager` 使用 Spring JDBC 或 iBatis 进行持久化数据时使用。
* `org.springframework.orm.hibernate5.HibernateTransactionManager` 使用 Hibernate 版本进行持久化数据使用。

#### 8.1.2 TransactionDefinition

是事务的定义信息对象：

* 获取事务对象名称：`String getName()`
* 获取事务隔离级：`int getIsolationLevel()`
* 获取事务传播行为：`int getPropagationBehavior()`
*  获取事务超时时间：`int getTimeout()`
* 获取事务是否只读：`boolean isReadOnly()`

读写型事务：增加、删除、修改开启事务。

只读型事务：执行查询时，也会开启事务。

##### 8.1.2.1 事务的隔离级别

事务的隔离级别反映事务提交并发访问时的处理态度：

* `ISOLATION_DEFAULT` 默认级别，归属下列某一种。
* `ISOLATION_READ_UNCOMMITTED` 可以读取未提交数据。
* `ISOLATION_READ_COMMITTED` 只能读取已经提交的数据，解决脏读问题（Oracle默认级别）
* 




#### 8.1.3 

### 8.2 Spring 事务控制的代码准备




### 8.3 Spring 基于 XML 的声明式事务控制



### 8.4 Spring 基于注解的声明式事务控制 



### 8.5 Spring 基于纯注解的声明式事务控制



### 8.6 Spring 编程式事务控制



### 8.7 Spring5 新特性的介绍
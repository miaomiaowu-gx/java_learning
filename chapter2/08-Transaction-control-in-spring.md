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

在开发中都是使用它的实现类 ：





### 8.2 Spring 事务控制的代码准备




### 8.3 Spring 基于 XML 的声明式事务控制



### 8.4 Spring 基于注解的声明式事务控制 



### 8.5 Spring 基于纯注解的声明式事务控制



### 8.6 Spring 编程式事务控制



### 8.7 Spring5 新特性的介绍
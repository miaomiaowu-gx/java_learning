## 第四节 

### 3.1 完善 account 案例

#### 3.1.1 案例中添加转账方法

基于 xml 案例代码，见本章第二节。

<img src="./img2/08-transfer.png" >


#### 3.1.2 分析事务的问题

<img src="./img2/09-transactionc-ontrol.png" >

#### 3.1.3 编写 ConnectionUtils

之前的例子，事务控制是在持久层，而实际上事务控制应该在业务层！

在src->main->java->com->itheima 下创建 `utils.ConnectionUtils` 类

  






#### 3.1.4 编写事务管理工具类并分析连接和线程解绑

 
#### 3.1.5 编写业务层和持久层事务控制代码并配置 Spring 的 ioc 

   
#### 3.1.6 测试转账并分析案例中的问题    



### 3.2 动态代理




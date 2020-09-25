## 第九节 Mybatis 连接池与事务控制

1、连接池：在实际开发中都会使用连接池，因为它可以减少获取连接所消耗的时间。

2、mybatis 连接池提供了 3 种方式的配置：

* 配置的位置：主配置文件 SqlMapConfig.xml 中的 dataSource 标签，type 属性就是表示采用何种连接池方式。

* type 属性的取值：
   * POOLED 采用传统的 javax.sql.DataSource 规范中的连接池，mybatis中有针对规范的实现
   * UNPOOLED 采用传统的获取连接的方式，虽然也实现Javax.sql.DataSource 接口，但是并没有使用池的思想。每次用重新创建连接。
   * JNDI 采用服务器提供的 JNDI 技术实现，来获取 DataSource 对象，不同的服务器所能拿到 DataSource 是不一样。
   * MyBatis 内部分别定义了实现了 java.sql.DataSource 接口的 UnpooledDataSource，PooledDataSource 类来表示 UNPOOLED、 POOLED 类型的数据源。
   
```xml
数据源配置就是在 SqlMapConfig.xml 文件中， 具体配置如下：
<!-- 配置数据源（连接池）信息 -->
<dataSource type="POOLED">
<property name="driver" value="${jdbc.driver}"/>
<property name="url" value="${jdbc.url}"/>
<property name="username" value="${jdbc.username}"/>
<property name="password" value="${jdbc.password}"/>
</dataSource>
MyBatis 在初始化时， 根据<dataSource>的 type 属性来创建相应类型的的数据源 DataSource，即：
type=”POOLED”： MyBatis 会创建 PooledDataSource 实例
type=”UNPOOLED” ： MyBatis 会创建 UnpooledDataSource 实例
type=”JNDI”： MyBatis 会从 JNDI 服务上查找 DataSource 实例，然后返回使用
```   
* 注意：如果不是 web 或者 maven 的 war 工程，是不能使用的。课程中使用的是 tomcat 服务器，采用连接池就是 dbcp 连接池。


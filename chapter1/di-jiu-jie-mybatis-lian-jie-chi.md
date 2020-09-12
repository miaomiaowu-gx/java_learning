## 第九节 Mybatis 连接池

1、连接池：在实际开发中都会使用连接池，因为它可以减少获取连接所消耗的时间。

2、mybatis 连接池提供了 3 种方式的配置：

* 配置的位置：主配置文件 SqlMapConfig.xml 中的 dataSource 标签，type 属性就是表示采用何种连接池方式。

* type 属性的取值：
   * POOLED 采用传统的javax.sql.DataSource规范中的连接池，mybatis中有针对规范的实现
   * UNPOOLED 采用传统的获取连接的方式，虽然也实现Javax.sql.DataSource 接口，但是并没有使用池的思想。每次用重新创建连接。
   * JNDI 采用服务器提供的 JNDI 技术实现，来获取 DataSource 对象，不同的服务器所能拿到 DataSource 是不一样。
   * MyBatis 内部分别定义了实现了 java.sql.DataSource 接口的 UnpooledDataSource，PooledDataSource 类来表示 UNPOOLED、 POOLED 类型的数据源。

* 注意：如果不是 web 或者 maven 的 war 工程，是不能使用的。课程中使用的是 tomcat 服务器，采用连接池就是 dbcp 连接池。

<hr>

在 IDEA 中，ctrl+N 弹出类搜索框，分别输入 `PooledDataSource` 与 `UnpooledDataSource` 。




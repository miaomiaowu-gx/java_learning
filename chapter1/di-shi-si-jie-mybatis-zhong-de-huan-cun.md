## 第十四节 Mybatis 中的缓存

**什么是缓存**
* 存在于内存中的临时数据。

**为什么使用缓存**
* 减少和数据库的交互次数，提高执行效率。

什么样的数据能使用缓存，什么样的数据不能使用缓存
* 适用于缓存：
  * **经常查询并且不经常改变的。**
  * 数据的正确与否对最终结果影响不大的。
* 不适用于缓存：
  * **经常改变的数据**
  * 数据的正确与否对最终结果影响很大的。
  * 例如：商品的库存，银行的汇率，股市的牌价。

Mybatis 提供了缓存策略，通过缓存策略来减少数据库的查询次数， 从而提
高性能。Mybatis 中缓存分为一级缓存，二级缓存。

* 一级缓存：
  * 它指的是 Mybatis 中 SqlSession 对象的缓存。
  * 当执行查询之后，查询的结果会同时存入到 SqlSession 为使用者提供一块区域中。该区域的结构是一个Map。当再次查询同样的数据，mybatis 会先去 sqlsession 中查询是否有，有的话直接拿出来用。
  * **当 SqlSession 对象消失时，mybatis 的一级缓存也就消失了**，即只要 SqlSession 没有 **flush 或 close 或 clear**，它就存在。
  * 一级缓存是 SqlSession 范围的缓存，当调用 SqlSession 的修改，添加，删除，commit()，close()等方法时，就会清空一级缓存。
	
* 二级缓存:
  * 它指的是 Mybatis 中 SqlSessionFactory 对象的缓存。由同一个SqlSessionFactory 对象创建的 SqlSession 共享其缓存。

* 二级缓存的使用步骤：
  * 第一步：让Mybatis框架支持二级缓存（在SqlMapConfig.xml中配置）
  * 第二步：让当前的映射文件支持二级缓存（在IUserDao.xml中配置）
  * 第三步：让当前的操作支持二级缓存（在select标签中配置）



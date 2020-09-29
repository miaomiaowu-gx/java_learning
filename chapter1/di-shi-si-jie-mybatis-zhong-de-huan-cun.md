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
  * 它指的是 Mybatis 中 SqlSessionFactory 对象的缓存。由同一个SqlSessionFactory 对象创建的 SqlSession 共享其缓存。二级缓存是 mapper 映射级别的缓存，多个 SqlSession 去操作同一个 Mapper 映射的 sql 语句，多个SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 的。

* 二级缓存的使用步骤：
  * 第一步：让Mybatis框架支持二级缓存（在SqlMapConfig.xml中配置）
  * 第二步：让当前的映射文件支持二级缓存（在IUserDao.xml中配置）
  * 第三步：让当前的操作支持二级缓存（在select标签中配置）

【SqlMapConfig.xml中配置】

因为 cacheEnabled 的取值默认就为 true，所以这一步可以省略不配置。为 true 代表开启二级缓存；为false 代表不开启二级缓存。

```xml
<settings>
  <!-- 开启二级缓存的支持 -->
  <setting name="cacheEnabled" value="true"/>
</settings>
```

【配置相关的 Mapper 映射文件】

```xml
<mapper namespace="com.itheima.dao.IUserDao">
  <!-- 开启二级缓存的支持 -->
  <cache></cache>
</mapper>
```

【配置 statement 上面的 useCache 属性】

将 UserDao.xml 映射文件中的<select>标签中设置 useCache=”true” 代表当前这个 statement 要使用二级缓存，如果不使用二级缓存可以设置为 false。

```xml
<!-- 根据 id 查询 -->
<select id="findById" resultType="user" parameterType="int" useCache="true">
  select * from user where id = #{uid}
</select>
```

注意：针对每次查询都需要最新的数据 sql，要设置成 useCache=false，禁用二级缓存。


**注意事项**

当使用二级缓存时，所缓存的类一定要实现 java.io.Serializable 接口，这种就可以使用序列化方式来保存对象。





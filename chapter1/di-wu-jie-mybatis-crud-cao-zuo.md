# 第五节 基于代理 Dao 实现 CRUD 操作


## 5.1 CRUD-保存操作

1）在 IUserDao 中，添加方法。

```java
void saveUser(User user);
```

2）在映射配置文件 IUserDao.xml 中添加映射。

```xml
<!-- 保存用户 parameterType告知函数接收参数类型-->
<!-- 实体类中，使用右键生成getter&setter，则获取值时，可直接使用实体类中定义的变量名-->
<!-- 如果实体类是自己实现的，则取值时写入get方法后的名字 -->
<insert id="saveUser" parameterType="com.itheima.domain.User">
    INSERT into user(username, address, sex, birthday) VALUES(#{username},#{address},#{sex},#{birthday});
</insert>
```

3）在测试类中添加新的测试方法

```java


```











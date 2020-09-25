## 第十节 Mybatis 的动态 SQL 语句

### 10.1 动态 SQL 之 if 标签

1 持久层 Dao 接口：在 IUserDao.java 文件中，添加：

```java
/**
 * 根据传入参数条件查询
 * @param user 查询条件：有可能有用户名、有可能有性别或其他，也可能是都有
 * @return
 */
List<User> findUserByCondition(User user);
```

2 在 `IUserDao.xml` 中添加配置：

```xml
<!-- 根据条件查询 -->
<select id="findUserByCondition" resultMap="userMap" parameterType="user">
    SELECT *  FROM USER WHERE 1=1
    <if test="userName != null">
        AND username = #{userName}
    </if>
    <if test="userSex != null">
        AND sex = #{userSex}
    </if>
</select>
```

3  测试

```java
@Test
public void testFindByCondition() {
    User user = new User();
    user.setUserName("老王");
    user.setUserSex("女");
    //5. 执行查询所有方法
    List<User> users = userDao.findUserByCondition(user);
    for(User u : users){
        System.out.println(u);
    }

}
```


### 10.2 动态 SQL 之 where 标签

为了简化上面 `where 1=1` 的条件拼装，可以采用 `<where>` 标签来简化开发。





### 10.3 



### 10.4 
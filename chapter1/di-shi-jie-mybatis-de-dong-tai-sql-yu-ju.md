## 第十节 Mybatis 的动态 SQL 语句

### 10.1 动态 SQL 之 if 标签

1. 持久层 Dao 接口：在 IUserDao.java 文件中，添加：

```java
/**
 * 根据传入参数条件查询
 * @param user 查询条件：有可能有用户名、有可能有性别或其他，也可能是都有
 * @return
 */
List<User> findUserByCondition(User user);
```

2. 在 `IUserDao.xml` 中添加配置：

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

3. 


### 10.2 



### 10.3 



### 10.4 
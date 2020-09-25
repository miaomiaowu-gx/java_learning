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

```xml
<!-- 根据条件查询 -->
<select id="findUserByCondition" resultMap="userMap" parameterType="user">
    SELECT *  FROM USER
    <where>
        <if test="userName != null">
            AND username = #{userName}
        </if>
        <if test="userSex != null">
            AND sex = #{userSex}
        </if>
    </where>
</select>
```

### 10.3 动态标签之 foreach 标签

需求 `select * from user where id in(41,41,43,46)`

1 在 QueryVo.java 中加入一个 List 集合用于封装参数:

```java
public class QueryVo {
    private List<Integer> ids;

    public List<Integer> getIds() {
        return ids;
    }

    public void setIds(List<Integer> ids) {
        this.ids = ids;
    }
}
```

2 持久层 Dao 接口：在 IUserDao.java 文件中，添加

```java
/**
 * 根据QueryVo中提供的id集合，查询用户信息
 * @param vo
 * @return
 */
List<User> findUserInIds(QueryVo vo);
```

3 持久层 Dao 映射配置：在 `IUserDao.xml` 中添加配置：

```java
<!-- 根据QueryVo中提供的id集合，查询用户信息 -->
<select id="findUserInIds" resultMap="userMap" parameterType="com.itheima.domain.QueryVo">
    SELECT * FROM USER
    <where>
        <if test="ids != null and ids.size()>0">
            <foreach collection="ids" open="and id in (" close=")" item="uid" separator=",">
                #{uid}
            </foreach>
        </if>
    </where>
</select>
```

4 测试




















### 10.4 
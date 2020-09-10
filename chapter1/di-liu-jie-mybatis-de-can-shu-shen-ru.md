# 第六节 Mybatis 的参数深入

## 6.1 Mybatis 的参数

### 6.1.1 parameterType 配置参数


### 6.1.2 传递 pojo 对象

Mybatis 使用 ognl 表达式解析对象字段的值，#{}或者${}括号中的值为 pojo 属性名称。

**OGNL 表达式**，即 Object Graphic Navigation Language，它通过对象的取值方法来获取数据。在写法上把 get 省略了。比如：获取用户的名称

* 类中的写法：`user.getUsername();`

* OGNL 表达式写法：`user.username`
 
那么 Mybatis 中为什么能直接写 `username`，而不用 `user.` ？：

&emsp;因为在 parameterType 中已经提供了属性所属的类，所以此时不需要写对象名。


### 6.1.3 传递 pojo 包装对象

开发中通过 pojo 传递查询条件，查询条件是**综合的查询条件**，不仅包括用户查询条件还包括其它的查询条件（比如将用户购买商品信息也作为查询条件），这时可以使用**包装对象**传递输入参数。

Pojo 类中包含 pojo。

功能：

1）在 IUserDao 中，添加方法。

```java
//根据queryVo中的条件查询用户
List<User> findUserByVo(QueryVo vo);
```

2）在 domain 文件夹下，创建 `QueryVo`类。

```java
package com.itheima.domain;

public class QueryVo {
    private User user;

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }
}
```

2）在映射配置文件 IUserDao.xml 中添加映射。




3）在测试类中添加新的测试方法




## 6.2 Mybatis 输出结果封装



## 6.3 resultMap 结果类型





1）在 IUserDao 中，添加方法。

2）在映射配置文件 IUserDao.xml 中添加映射。


3）在测试类中添加新的测试方法





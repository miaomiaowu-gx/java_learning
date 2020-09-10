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


## 6.2 Mybatis 输出结果封装



## 6.3 resultMap 结果类型
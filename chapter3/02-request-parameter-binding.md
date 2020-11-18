## 第二节 请求参数绑定

#### 2.1 绑定的机制 

表单中请求参数都是基于 key=value，SpringMVC 绑定请求参数的过程是通过把表单提交请求参数，作为控制器中方法参数进行绑定的。 

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>请求参数</h3>
    <a href="param/testParam?username=haha&password=1234">请求参数</a>
</body>
</html>
```

```java
@Controller
@RequestMapping("/param")
public class ParamController {
    @RequestMapping("/testParam")
    //参数 username、password 与请求 url 中的参数一致！否则获取不到值。
    public String testRequestMapping(String username, String password){
        System.out.println("请求参数绑定 ...");
        System.out.println("用户名： "+username);
        System.out.println("密码： "+password);
        return "success";
    }
}
```

**支持的数据类型：**

* **基本类型参数**：包括基本类型和 String 类型，**要求参数名称必须和控制器中方法的形参名称保持一致。 (严格区分大小写)**
* POJO 类型参数：包括**实体类**，以及关联的实体类，要求表单中参数名称和 POJO 类的属性名称保持一致。并且控制器方法的参数类型是 POJO 类型。
* **数组和集合类型参数**：包括 List 结构和 Map 结构的集合（包括数组） 

#### 2.2 请求参数绑定实体类型

**实体类**

```java
public class User implements Serializable{
    private String uname;
    private Integer age;
    getter & setter 方法...
}

public class Account implements Serializable{
    private String username;
    private String password;
    private Double money;
    private User user;
}    
```

**提交表单**

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>请求参数</h3>
    <%--<a href="param/testParam?username=haha&password=1234">请求参数</a>--%>

    <%--把数据封装Account类中--%>
    <form action="param/saveAccount" method="post">
        <%-- name 属性，与实体类中属性名相同 --%>
        姓名：<input type="text" name="username" /><br/>
        密码：<input type="text" name="password" /><br/>
        金额：<input type="text" name="money" /><br/>
        用户姓名：<input type="text" name="user.uname" /><br/>
        用户年龄：<input type="text" name="user.age" /><br/>
        <input type="submit" value="提交" />
    </form>
</body>
</html>
```

**Controller**

```java
@Controller
@RequestMapping("/param")
public class ParamController {
    /**
     * 请求参数绑定把数据封装到JavaBean的类中
     * @return
     */
    @RequestMapping("/saveAccount")
    //参数为实体类类型 Account 
    public String saveAccount(Account account){
        System.out.println("执行了...");
        System.out.println(account);
        return "success";
    }
}
```

#### 2.3 配置解决中文乱码的过滤器

配置过滤器拦截，解决中文乱码问题。在 web.xml 中添加过滤器配置：

```xml
<!--配置解决中文乱码的过滤器-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

#### 2.4 请求参数绑定集合类型

**实体类**

```java
public class Account implements Serializable{
    private String username;
    private String password;
    private Double money;

    private List<User> list;
    private Map<String,User> map;
    
    ...
}    
```

**提交表单**

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>请求参数</h3>

    <%--把数据封装Account类中，类中存在list和map的集合--%>
    <form action="param/saveAccount" method="post">
        姓名：<input type="text" name="username" /><br/>
        密码：<input type="text" name="password" /><br/>
        金额：<input type="text" name="money" /><br/>
		
        用户姓名：<input type="text" name="list[0].uname" /><br/>
        用户年龄：<input type="text" name="list[0].age" /><br/>

        用户姓名：<input type="text" name="map['one'].uname" /><br/>
        用户年龄：<input type="text" name="map['one'].age" /><br/>
        <input type="submit" value="提交" />
    </form>
</body>
</html>
```

#### 2.5 自定义类型转换器

##### 2.5.1 问题描述-文本框输入含有日期对象

**实体类**

```java
public class User implements Serializable{
    private String uname;
    private Integer age;
    private Date date;
}
```

**提交表单**

```jsp
<%--自定义类型转换器--%>
<form action="param/saveUser" method="post">
    用户姓名：<input type="text" name="uname" /><br/>
    用户年龄：<input type="text" name="age" /><br/>
    用户生日：<input type="text" name="date" /><br/>
    <input type="submit" value="提交" />
</form>
```

**Controller**

```java
@Controller
@RequestMapping("/param")
public class ParamController {
    // 自定义类型转换器
    @RequestMapping("/saveUser")
    public String saveUser(User user){
        System.out.println("执行了...");
        System.out.println(user);
        return "success";
    }
}
```

当用户生日输入 `2020/11/11` 时，可正常访问。然而，当输入 `2020-11-11` 会**报 400-Bad Request 错误**。

##### 2.5.2 解决方案

**分析**：在表单处输入的任意数据都会作为字符串提交，如用户年龄值 99，SpringMvc 会将该字符串转为 Integer 类型。而输入的 2020/11/11，SpringMvc 无法将其转换为 Date 类型。

**解决方案**：自定义类型转换器

1 **定义一个类，实现 Converter 接口，该接口有两个泛型**。

* 在 java 文件夹下创建 cn.itcast.utils.StringToDateConverter 日期转换类

```java
package cn.itcast.utils;

import org.springframework.core.convert.converter.Converter;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
/**
 * 把字符串转换日期
 * Converter<S, T>：
 *  第一个参数 S 固定为字符串（页面以字符串形式传递所有数据）
 *  第二个参数 T 为要转换的类型
 */
public class StringToDateConverter implements Converter<String, Date>{
    /**
     * String source    传入进来字符串
     */
    @Override
    public Date convert(String source) {
        // 判断
        if(source == null){
            throw new RuntimeException("请您传入数据");
        }
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd");

        try {
            // 把字符串转换日期
            return df.parse(source);
        } catch (Exception e) {
            throw new RuntimeException("数据类型转换出现错误");
        }
    }
}
```

2 在 springmvc.xml 文件中添加**自定义类型转换器配置**，将自定义的转换器注册到类型转换服务中去。 

3 在 **annotation-driven** 标签中**引用**配置的类型转换服务。 

```xml
<!--配置自定义类型转换器-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="cn.itcast.utils.StringToDateConverter"/>
        </set>
    </property>
</bean>

<!-- 开启SpringMVC框架注解的支持-->
<mvc:annotation-driven conversion-service="conversionService"/>
```

#### 2.6 获取Servlet原生的API

1 超链接

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>Servlet原生的API</h3>
    <a href="param/testServlet">Servlet原生的API</a>
</body>
</html>
```

2 想获取什么对象，就在 Controller 函数参数中添加对应的变量

```java
package cn.itcast.controller;

import cn.itcast.domain.Account;
import cn.itcast.domain.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@Controller
@RequestMapping("/param")
public class ParamController {
    // 原生的API
    @RequestMapping("/testServlet")
    public String testServlet(HttpServletRequest request, HttpServletResponse response){
        System.out.println("执行了...");
        System.out.println(request);

        HttpSession session = request.getSession();
        System.out.println(session);

        ServletContext servletContext = session.getServletContext();
        System.out.println(servletContext);

        System.out.println(response);
        return "success";
    }
}
```

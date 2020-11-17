## 第一节 SpringMVC 入门

### 1.1 入门程序

#### 1.1.1 需求分析

<img src="./img3/02-Introductory-cases.png" width=300>

#### 1.1.2 搭建开发环境

1 创建新工程，选择 Maven 并勾选 Create from archetype，选择 org.apache.maven.archetypes:maven-archetype-webapp。GroupId 设为 cn.itcast，Artifactld 设为 `springmvc_quick_start`。

* 解决项目创建过慢问题，添加键值对`archetypeCatalog : internal`。

2 在 main 文件夹下创建 java 文件夹，右键选择 Mark Directory as，选择 Sources Root。在 main文件夹下创建 resources 文件夹，右键选择 Mark Directory as，选择 Resources Root。

3 导入需要的坐标，在 pom.xml 中配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>cn.itcast</groupId>
  <artifactId>springmvc_quick_start</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>springmvc_quick_start Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <!-- 版本锁定 -->
    <spring.version>5.0.2.RELEASE</spring.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.0</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>

  <build>
    <finalName>springmvc_quick_start</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```

4 在 web.xml 中**配置前端控制器**（实质就是一个 Servlet）

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

5 在 resources 文件夹下创建 XML Configuration File->Spring Config->springmvc 文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

6 部署服务器，在 Run/Debug Configurations 页面添加一个 Local Tomcat Server，命名为 springmvc。在 Deployment 选项点击➕，选择 Artifact，选择 springmvc_quick_start:war。

#### 1.1.3 代码编写

1 在 webapp 下创建 index.jsp 文件（主页面）

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>入门程序</h3>
    <a href="/hello">入门程序</a>
</body>
</html>
```

2 在 java 文件夹下创建 cn.itcast.controller.HelloController 类

```java
package cn.itcast.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

// 控制器类
@Controller
public class HelloController {
    @RequestMapping("/hello")  
    public String sayHello(){
        System.out.println("Hello StringMVC");
        //返回值字符串的名字，是跳转后的文件名
        return "success";  //跳转到 success.jsp 文件
    }
}

/*
@Controller：定义了一个控制器类，使用它标记的类就是一个 SpringMvc Controller 对象，分发处理器会扫描使用该注解的类的方法，并检测该方法是否使用了 @RequestMapping 注解。
* @Controller 只是定义了一个控制器类，而使用 @RequestMapping 注解的方法才是处理请求的处理器。
* @Controller 标记在一个类上还不能真正意义上说它就是 SpringMvc 的控制器，因为这个时候 Spring 还不认识它，需要把这个控制器交给 Spring 来管理。有两种方式可以管理：

<!--基于注解的装配-->

<!--方式一-->
<bean class="com.HelloWorld"/>

<!--方式二-->
<!--路径写到controller的上一层-->
<context:component-scan base-package="com"/>
*/
```


3 在 WEB-INF 中创建 pages文件夹，在 pages文件夹创建 success.jsp 文件

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>入门成功</h3>
</body>
</html>
```

4 更新 springmvc.xml 文件约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 开启注解扫描 -->
    <context:component-scan base-package="cn.itcast"/>

    <!-- 视图解析器对象 帮助跳转指定页面-->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- prefix：文件路径 -->
        <property name="prefix" value="/WEB-INF/pages/"/>
        <!-- suffix：文件后缀名 -->
        <property name="suffix" value=".jsp"/>
    </bean>

    <!-- 开启 SpringMVC 框架注解的支持 -->
    <mvc:annotation-driven/>
</beans>
```

 5 在 web.xml 中加载配置文件

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <!-- 使用前端控制器加载配置文件 -->
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

**load-on-startup 标签作用**：

1. load-on-startup 元素标记容器**是否在启动**的时候就加载这个 servlet (实例化并调用其 init() 方法)。

2. 它的值必须是一个整数，表示 servlet 应该被载入的**顺序**。

3. 当值为 0 或者大于 0 时，表示容器在启动时就加载并初始化这个 servlet。

4. 当值小于 0 或者没有指定时，则表示容器在该 Servlet 第一次被请求时，才会去加载。

5. 正数的值越小，该 Servlet 的优先级就越高，应用启动时就优先加载。**数字代表的是优先级，而非启动延迟时间。**

6. 当值相同的时候，容器就会自己选择优先加载。

#### 1.1.4 流程总结

<img src="./img3/03-pipline.png" width=600>

**执行过程**

1. 服务器启动，应用被加载。读取到 web.xml 中的配置创建 spring 容器并且初始化容器中的对象。

2. 浏览器发送请求，被 DispatherServlet 捕获，该 Servlet 并不处理请求，而是把请求转发出去。转发的路径是根据请求 URL，匹配 @RequestMapping 中的内容。

3. 匹配到了后，执行对应方法。该方法有一个返回值。

4. 根据方法的返回值，借助 InternalResourceViewResolver 找到对应的结果视图。

5. 渲染结果视图，响应浏览器。

**注意**

【web.xml】由于配置了 load-on-startup 标签，当服务器启动时，会立即创建 DispatcherServlet 对象，并加载 springmvc.xml 配置文件。

【springmvc.xml】开启注解扫描，将 HelloController 类变为对象（默认单例）加载进 IOC 容器中。

#### 1.1.5 使用的组件介绍


<img src="./img3/04-component-execution-process.png" width=1300>

**DispatcherServlet：前端控制器**

* 用户请求到达前端控制器，它就相当于 mvc 模式中的 c， dispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求， dispatcherServlet 的存在降低了组件之间的耦合性。

**HandlerMapping：处理器映射器**

* HandlerMapping 负责根据用户请求找到 Handler 即处理器， SpringMVC 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

**Handler：处理器** 

* 开发中要编写的具体业务控制器。由 DispatcherServlet 把用户请求转发到 Handler。由 Handler 对具体的用户请求进行处理。 

**HandlAdapter：处理器适配器**

* 通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

**View Resolver：视图解析器**

* View Resolver 负责将处理结果生成 View 视图， View Resolver 首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。

**View：视图**

* SpringMVC 框架提供了很多的 View 视图类型的支持，包括： jstlView、freemarkerView、pdfView 等。**最常用的视图就是 jsp**。一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。

🍒 **`<mvc:annotation-driven>` 说明** 🍒

&emsp;&emsp;在 SpringMVC 的各个组件中，**处理器映射器、处理器适配器、视图解析器**称为 SpringMVC 的**三大组件**。使 用 `<mvc:annotation-driven>` 自 动 加 载 `RequestMappingHandlerMapping`（ 处 理 映 射 器 ） 和 `RequestMappingHandlerAdapter`（ 处 理 适 配 器 ），可 用 在 springmvc.xml 配 置 文 件 中 使 用 `<mvc:annotation-driven>` **替代**注解处理器和适配器的配置。 

`<mvc:annotation-driven>` 相当于如下配置：

```xml
<!-- 上面的标签相当于 如下配置-->
<!-- Begin -->
<!-- HandlerMapping -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean>
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>

<!-- HandlerAdapter -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean>
<bean class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter"></bean>
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>

<!-- HadnlerExceptionResolvers -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExcept
ionResolver"></bean>
<bean class="org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolv
er"></bean>
<bean class="org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver"
></bean>
<!-- End -->
```

#### 1.1.6 RequestMapping 注解 

**作用**：用于建立请求 URL 和处理请求方法之间的对应关系。 

**源码**：

```java
@Target({ElementType.METHOD, ElementType.TYPE}) //元注解：可作用在方法、作用在类上
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Mapping
public @interface RequestMapping {
    String name() default "";

    @AliasFor("path")
    String[] value() default {};

    @AliasFor("value")
    String[] path() default {};

    RequestMethod[] method() default {};
    String[] params() default {};
    String[] headers() default {};
    String[] consumes() default {};
    String[] produces() default {};
}

/*
@Retention 按生命周期来划分可分为 3 类：
1、RetentionPolicy.SOURCE：注解只保留在源文件，当Java文件编译成class文件的时候，注解被遗弃；
2、RetentionPolicy.CLASS：注解被保留到class文件，但jvm加载class文件时候被遗弃，这是默认的生命周期；
3、RetentionPolicy.RUNTIME：注解不仅被保存到class文件中，jvm加载class文件之后，仍然存在；
这3个生命周期分别对应于：Java源文件(.java文件) ---> .class文件 ---> 内存中的字节码。
*/
```

**出现位置**： 

* 类上：请求 URL 的第一级访问目录。此处不写的话，就相当于应用的根目录。 写的话需要以 `/` 开头。它出现的目的是为了使 URL 可以按照模块化管理: 

```
账户模块：
/account/add
/account/update
/account/delete

订单模块：
/order/add
/order/update
/order/delete

/account 与 /order 就是把 RequsetMappding 写在类上，使 URL 更加精细。
```

* 方法上：请求 URL 的第二级访问目录。

**属性**： 

1. `value`：用于指定请求的 URL。 它和 path 属性的作用是一样的。

2. `method`：用于指定请求的方式。取值有：RequestMethod.GET、RequestMethod.POST、RequestMethod.DELETE、RequestMethod.HEAD 等。

3. `params`：用于指定限制请求参数的条件。它支持简单的表达式。 要求请求参数的 key 和 value 必须和配置的一模一样。

    ```html
    params = {"accountName"}，表示 url 请求参数必须有 accountName，例：
    <a href="/testRequestMapping?accountName">RequestMapping 注解</a>
    
    params = {"moeny=100"}，表示请求参数中 money 必须是 100，例：
    <a href="/testRequestMapping?moeny=100">RequestMapping 注解</a>
    ```
    
4. `headers`：用于指定限制请求消息头的条件。

注意：以上四个属性只要出现 2 个或以上时，他们的关系是与的关系。 

#### 1.1 7 配置注意事项

🍒 **在 jsp 中使用第二种方法配置时，不要在访问 URL 前面加 `/`，否则无法找到资源。** 

```html
<!-- 第一种访问方式 -->
<a href="${pageContext.request.contextPath}/account/findAccount">查询账户</a>
<br/>
<!-- 第二种访问方式 -->
<a href="account/findAccount">查询账户</a>
```

### 1.2 请求参数的绑定

#### 1.2.1 绑定的机制 

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

#### 1.2.2 请求参数绑定实体类型

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

#### 1.2.3 配置解决中文乱码的过滤器

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

#### 1.2.4 请求参数绑定集合类型

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

#### 1.2.5 自定义类型转换器

##### 1.2.5.1 问题描述-文本框输入含有日期对象

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

##### 1.2.5.2 解决方案

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

#### 1.2.6 获取Servlet原生的API

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





  











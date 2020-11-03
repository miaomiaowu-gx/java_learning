## 第十二节 Servlet

### 12.1 Servlet 概述

概念：运行在 web 服务器端的小程序
   * Servlet 就是一个接口，定义了 Java 类被浏览器访问到(tomcat 识别)的规则。
   * 将来自定义一个类，实现 Servlet 接口，复写方法。

### 12.2 Servlet 快速入门

1. 创建 JavaEE 项目

2. 定义一个类，实现 Servlet 接口，并实现接口中所有的抽象方法。
   * 在 src 文件夹下创建包 `cn.itcast.web.servlet`，创建 `ServletDemo1` 类。
   * 实现 Servlet 接口，并实现接口中的方法。 

```java
package cn.itcast.web.servlet;

import javax.servlet.*;
import java.io.IOException;

/**
 * 快速入门
 */
public class ServletDemo1 implements Servlet{
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }
    //提供服务的方法
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Hello, Servlet!");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

3. 配置 Servlet，修改 web->WEB-INF 下的 web.xml 文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <!--配置 Servlet-->
    <servlet>
        <servlet-name>demo1</servlet-name>
        <servlet-class>cn.itcast.web.servlet.ServletDemo1</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>demo1</servlet-name>
        <!--将来访问的资源路径-->
        <url-pattern>/demo</url-pattern>
    </servlet-mapping>
</web-app>
```




### 12.3 Servlet 执行原理

 
  
   
### 12.4 Servlet 生命周期

      
            
                  
### 12.5 Servlet 3.0 注解配置 

### 12.6 IDEA 与 tomcat 相关配置

### 12.7

### 12.8     


                           
                                                      
                                                                                                             
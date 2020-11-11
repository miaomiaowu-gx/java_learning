## 第十八节 Filter Listener

### 18.1 Filter 过滤器

#### 18.1.1 Filter 介绍

&emsp;&emsp;web 中的过滤器：当访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能。

&emsp;&emsp;过滤器的作用：一般用于完成通用的操作。如：登录验证、统一编码处理、敏感字符过滤...

#### 18.1.2 Filter 快速入门

步骤：

1. 定义一个类，实现接口 Filter

2. 复写方法

3. 配置拦截路径
   * web.xml
   * 注解

【代码实现】

```java
package cn.itcast.web.filter;

import javax.servlet.*;
import java.io.IOException;

/**
 * 过滤器快速入门程序
 */
//@WebFilter("/*")//访问所有资源之前，都会执行该过滤器
public class FilterDemo1 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("filterDemo1被执行了....");
        //放行
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {
    }
}
```

一定要放行，否则不能访问到要访问的资源。


#### 18.1.3 过滤器细节

**1) web.xml 配置**	

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <filter>
        <filter-name>demo1</filter-name>
        <filter-class>cn.itcast.web.filter.FilterDemo1</filter-class>
    </filter>
    
    <filter-mapping>
        <filter-name>demo1</filter-name>
        <!-- 拦截路径 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```


**2) 过滤器执行流程**

1. 执行过滤器
2. 执行放行后的资源
3. 回来执行过滤器放行代码下边的代码

```java
public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
    //对request对象请求消息增强(1. 执行过滤器)
    System.out.println("filterDemo2执行了....");
    //放行(2. 执行放行后的资源)
    chain.doFilter(req, resp);
    //对response对象的响应消息增强(3. 回来执行过滤器放行代码下边的代码)
    System.out.println("filterDemo2回来了...");
}
```


**3) 过滤器生命周期方法**

1. init：在服务器启动后，会创建 Filter 对象，然后调用 init 方法。只执行一次。用于加载资源。
2. doFilter：每一次请求被拦截资源时，会执行。可执行多次。
3. destroy：在服务器关闭后，Filter 对象被销毁。如果服务器是正常关闭，则会执行 destroy 方法。只执行一次。用于释放资源。

**4) 过滤器拦截路径配置详解**

🍒 **拦截路径配置**

1. 具体资源路径：`/index.jsp`。只有访问 index.jsp 资源时，过滤器才会被执行。
2. 拦截目录：`/user/*`。	访问 /user 下的所有资源时，过滤器都会被执行。
3. 后缀名拦截：`*.jsp`。访问所有后缀名为jsp资源时，过滤器都会被执行。
4. 拦截所有资源：`/*`。访问所有资源时，过滤器都会被执行。

🍒 **拦截方式配置**：资源被访问的方式

注解配置：设置 dispatcherTypes 属性（是一个数组，可以配置多个值）

1. REQUEST：默认值。浏览器直接请求资源
2. FORWARD：转发访问资源
3. INCLUDE：包含访问资源
4. ERROR：错误跳转资源
5. ASYNC：异步访问资源

```java
@WebFilter(value="/*",dispatcherTypes ={ DispatcherType.FORWARD,DispatcherType.REQUEST})
```

web.xml 配置：设置在 `<filter-mapping>` 内部配置 `<dispatcher></dispatcher>` 标签即可。

**5) 过滤器链(配置多个过滤器)**

**执行顺序**：如果有两个过滤器：过滤器 1 和过滤器 2

1. 过滤器 1
2. 过滤器 2
3. 资源执行
4. 过滤器 2
5. 过滤器 1 
	

**过滤器先后顺序问题**：

1. 注解配置：按照类名的字符串比较规则比较，值小的先执行。如： AFilter 和 BFilter，AFilter 就先执行了。

2. web.xml 配置：`<filter-mapping>` 中谁定义在上边，谁先执行。

#### 18.1.4 案例 1 登录验证

需求(最基础的权限控制)：

1. 访问一个 web 项目的资源。先验证其是否登录。

2. 如果登录了，则直接放行。

3. 如果没有登录，则跳转到登录页面，提示"您尚未登录，请先登录"。
				

> 登录相关的资源：login.jsp 页面；显示 jsp 的所有东西都是相关的，如验证码；CSS、JS 样式；以及点击登录按钮后访问的 LoginServlet 是登录相关的资源。**对于这些资源，不能进行登录拦截，否则会形成死循环**（当前没有登录，需要的登录，然而登录页面需要已经登录才能访问...）

分析：

1. 判断是否是登录相关的资源
  * 是：直接放行
  * 不是：判断是否登录

2. 判断当前用户是否登录，判断 Session 中是否有 User
  * 有：已经登录，放行
  * 没有：没有登录，则跳转到登录界面 

【代码】

```java
package cn.itcast.web.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

/**
 * 登录验证的过滤器
 */
@WebFilter("/*")
public class LoginFilter implements Filter {

    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        System.out.println(req);
        //0.强制转换
        HttpServletRequest request = (HttpServletRequest) req;
        //1.获取资源请求路径
        String uri = request.getRequestURI();
        //2.判断是否包含登录相关资源路径,要注意排除掉 css/js/图片/验证码等资源
        if(uri.contains("/login.jsp") || uri.contains("/loginServlet") || uri.contains("/css/") || uri.contains("/js/") || uri.contains("/fonts/") || uri.contains("/checkCodeServlet")  ){
            //包含，用户就是想登录。放行
            chain.doFilter(req, resp);
        }else{
            //不包含，需要验证用户是否登录
            //3.从获取session中获取user
            Object user = request.getSession().getAttribute("user");
            if(user != null){
                //登录了。放行
                chain.doFilter(req, resp);
            }else{
                //没有登录。跳转登录页面
                request.setAttribute("login_msg","您尚未登录，请登录");
                request.getRequestDispatcher("/login.jsp").forward(request,resp);
            }
        }
        // chain.doFilter(req, resp);
    }

    @Override
    public void init(FilterConfig config) throws ServletException {
    }

    @Override
    public void destroy() {
    }
}
```

#### 18.1.5 案例 2 敏感词汇过滤

需求：

1. 对 day17_case 案例录入的数据进行敏感词汇过滤

2. 敏感词汇参考《敏感词汇.txt》

3. 如果是敏感词汇，替换为 *** 

分析：

1. 对 request 对象进行增强。增强获取参数相关方法

2. 放行。传递代理对象。




### 18.2 Listener 监听器 

概念：web 的三大组件之一。

事件监听机制
* 事件	：一件事情
* 事件源 ：事件发生的地方
* 监听器 ：一个对象
* 注册监听：将事件、事件源、监听器绑定在一起。 当事件源上发生某个事件后，执行监听器代码
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

1) web.xml 配置	

2) 过滤器执行流程


3) 过滤器生命周期方法



4) 过滤器配置详解



5) 过滤器链(配置多个过滤器)


#### 18.1.4 案例 1 登录验证

需求：

1. 访问 day17_case 案例的资源。验证其是否登录。

2. 如果登录了，则直接放行。

3. 如果没有登录，则跳转到登录页面，提示"您尚未登录，请先登录"。
				
				
				
				
#### 18.1.5 案例 2 敏感词汇过滤

需求：

1. 对 day17_case 案例录入的数据进行敏感词汇过滤

2. 敏感词汇参考《敏感词汇.txt》

3. 如果是敏感词汇，替换为 *** 

分析：

1. 对 request 对象进行增强。增强获取参数相关方法

2. 放行。传递代理对象。




### 18.2 Listener 监听器 
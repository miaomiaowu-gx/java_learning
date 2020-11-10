## 第十七节 EL & JSTL

### 17.1 JSP

#### 17.1.1 JSP 指令

作用：用于配置 JSP 页面，导入资源文件

格式：`<%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>`

分类：

1）page：配置 JSP 页面的

```html
<%@ page import="java.util.List" %>
<%@ page contentType="text/html;charset=gbk" errorPage="500.jsp"   pageEncoding="GBK" language="java" buffer="16kb" %>
```

* contentType：等同于 `response.setContentType()`
   * 设置响应体的 mime 类型以及字符集。
   * 设置当前 jsp 页面的编码（只能是高级的 IDE 才能生效；如果使用低级工具，则需要设置 pageEncoding 属性指定当前页面的字符集）。

* import：导包。

* errorPage：当前页面发生异常后，会自动跳转到指定的错误页面。

* isErrorPage：标识当前也是是否是错误页面。
   * true：是，可以使用内置对象 exception
   * false：否。默认值。不可以使用内置对象 exception

```html
<%
    String message = exception.getMessage();
    out.print(message);
%>
```

* buffer：缓冲区，默认 8kb。

2）include：包含页面，导入页面的资源文件

* `<%@include file="top.jsp"%>`


3）taglib：导入资源

* `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`

* prefix：前缀，自定义名字

* 使用 `<c:cathch ... >`


#### 17.1.2 注释

1. html 注释：会发送到浏览器端
   * `<!-- -->`：只能注释 html 代码片段
   
2. jsp 注释：推荐使用，不会发送到浏览器端
   * `<%-- --%>`：可以注释所有

```jsp
<!--
    可以注释标签，但不能注释由 <% %>包围的代码
    <h1>hello</h1>
-->
     
<%--
    该注释既可以注释代码也可以注释标签
    <%
        System.out.println("hi~~~~");
    %>
    
    <input>
--%>
```

#### 17.1.3 内置对象

在 jsp 页面中不需要创建，直接使用的对象，一共有 9 个，前 4 个是域对象，用于共享数据：

|变量名|真实类型|作用|
|:--:|:--:|----|
|pageContext|PageContext|当前页面共享数据，还可以获取其他八个内置对象|
|request|HttpServletRequest|一次请求访问的多个资源(转发)|
|session|HttpSession|一次会话的多个请求间|
|application|ServletContext|所有用户间共享数据(范围最大)，唯一，服务器开启其被创建，服务器关闭，其被销毁|
|response|HttpServletResponse|响应对象|
|page|Object|当前页面(Servlet)的对象  this|
|out|JspWriter|输出对象，数据输出到页面上|
|config|ServletConfig|Servlet 的配置对象|
|exception|Throwable|异常对象|

```html
<%
pageContext.setAttribute("msg","hello");
%>

<%=pageContext.getAttribute("msg")%>
```

记住内置对象的名字，题目：请写出 JSP 的 9 个内置对象？

### 17.2 MVC 开发模式

**1）jsp 演变历史**

1. 早期只有 servlet，只能使用 response 输出标签数据，非常麻烦

2. 后来有 jsp，简化了 Servlet 的开发，如果过度使用 jsp，在 jsp 中既写大量的 java 代码，又写 html 标签，造成难于维护，难于分工协作。 

3. 再后来，java 的 web 开发，借鉴 mvc 开发模式，使得程序的设计更加合理性。
	
**2）MVC**

1. M：Model，模型。【JavaBean】。完成具体的业务操作，如：查询数据库，封装对象。

2. V：View，视图。【JSP】。展示数据。

3. C：Controller，控制器。【Servlet】。
	* 获取用户的输入
	* 调用模型
	* 将数据交给视图进行展示

<img src="./img6/82-MVC-mode.png" width=600>



**3）优缺点**

优点：耦合性低，方便维护，可以利于分工协作；重用性高。
	
缺点：使得项目架构变得复杂，对开发人员要求高


### 17.3 EL 表达式






### 17.4 JSTL 标签






### 17.5 三层架构






